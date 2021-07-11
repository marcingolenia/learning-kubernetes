## Config maps
Provide a wait to store configuration information and provide it to containers. Can store entire files (filename is key, content is value) or provide key/value pairs.

ConfigMaps can be accessed from a Pod using: 
- Env variables
- ConfigMap Volume (access as files)

this can be used with kubectl apply/create:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-settings
  labels:
    app: app-settings
data:
  enemies: aliens
  lives: "3"
  enemies.cheat: "true"
  enemies.cheat.level: noGoodRotten
```

`kubectl get cm [cm-name] -o yaml` get config map and view its contents.
Here's how config maps can be used:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-configmap
spec:
  selector:
    matchLabels:
      app: node-configmap
  template:
    metadata:
      labels:
        app: node-configmap
    spec:
      containers:
      - name: node-configmap
        image: node-configmap
        imagePullPolicy: IfNotPresent
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 9000

        # Reference ConfigMap data at appropriate path (/etc/config)
        volumeMounts:
          - name: app-config-vol
            mountPath: /etc/config

        # Map env variable data to a ConfigMap property
        env:
        - name: ENEMIES
          valueFrom:
            configMapKeyRef:
              name: app-settings
              key: enemies
              # optional: true    # Can mark a ConfigMap reference as optional

        # Load all ConfigMap keys/values into environment variables (simplified way if you need them all vs. only a few as with "env" above)
        envFrom:
        - configMapRef:
            name: app-settings

      # Define volume that will contain ConfigMap data
      volumes:
      - name: app-config-vol
        configMap:
          name: app-settings
```

## Secrets
Kubernetes can store sensitive information that avoids storing secrets in container images, inf files or in deployment manifest.

Secrets can be mount into pods as files or env. They are stored in tmpfs on a Node (not on disk).

1. Enbale encruption at rest for cluster data
2. Limit access to etcd (where Secrets are stored) to only admin users
3. Use SSL/TLS for etcd peer-to-peer communication.
4. Manifest (YAML/JSON) files only base64 encode the Secret.
5. Pods can access Secrets so secure which users can create Pods. Rolbe-based access control can be used.

https://kubernetes.io/docs/concepts/configuration/secret/

Example:

```
kubectl create secret generic db-passwords --from-literal=db-password='password' --from-literal=db-root-password='password'
```

Example of cert:
```
kubectl create secret tls tls-secret --cert=path/to/tls.cert --key=path/to/tls,key 
```

`kubectl get secrets` lists secrets
`kubectl get secrets db-passwords -o yaml` get secret with value (in base64)! admin should shut down this command.

using secrets:
```yaml
apiVersion: apps/v1
...
spec: 
    template:
        ...
    spec:
    containers: ...
    env:
        - name: DATABASE_PASSWORD
          valueFrom:
            secretKeyRef:
                name: db-passwords
                key: db-password
```