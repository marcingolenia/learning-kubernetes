Deployments and ReplicaSets ensure Pods stay running and can be used to scale Pods.

Replica Sets
- act as a Pod controller
- Self healing mechanism
- Ensures that the requested number of Pods are availible
- Provide fault tolerance
- Can be sued to scale Pods
- Relies on a Pod template
- Used by deployments

Deployment manages Pods:
- Pods are managed using RepllicaSets
- Scales ReplicaSets which scale Pods
- Supports zero-downtime updates by creating and destroying ReplicaSets
- Provides rollback functionality
- Creates a unique labael that is assigned to the ReplicaSet and generatedPods
- YAML is very similar to ReplicaSet

## Example of deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: frontend
  labels: 
    app: my-nginx
    tier: frontend
spec: 
  replicas: 1
  selector: # Selector is used to "select" the teamplate to use (based on labels)
    matchLabels:
      tier: frontend
  template:
    metadata:
      lagels:
        tier: frontend # Teamplate to use to create Pod/Containers
    spec: 
      containers: # Everything related to container like healing.
        - name: my-nginx
          image: nginx:alpine
          ports: 
          - containerPort: 80
          resources:
            limits:
              memory: "128Mi" #128MB
              cpu: "200m" #200 millicpu (.2cpu or 20% of the cpu)
```

For commands related to deployments see commands in [1_theory.md](./1_theory.md).