## There are 2 types of probes
- Liveness probe // When should a container restart?
- Readiness probe // When should a container start receiving traffic?

Failed Pod containers are recreated by deault (restartPolicy is always)

HTTP Healthcheck definition (we may actually write some healthchecks here)
```yaml
apiVersion: v1
kind: Pod
...
spec: 
    containers:
    - name : my-nginx
      image : nginx:alpine
      livenessProbe:
        httpGet:
          path: /index.html
          port: 80
        initialDelaySeconds: 15
        timeoutSeconds: 2
        periodSeconds: 5
        failureThreshold: 1
      readinessPod:
        httpGet:
          path: /index.html
          port: 80
        initialDelaySeconds: 2
        periodSeconds: 5
```

Exec Action healthcheck - example on kubert docs;
https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/. 
