Repo for course:
https://github.com/DanWahlin/DockerAndKubernetesCourseCode

## 1. Key kubernetes features
- Service Discovery / Load balancing
- Sotarage Orchestration
- Automate Rollouts/Rollbacks
- Self-healing
- Secret and Configuration Managment
- Horizontal Scaling

## 2. Big picture
Kubernetess has one or more master nodes that knows how to manage rest of the nodes (worker nodes which are usually VMs). Pods are boxes for containers (space suits and astronaut metaphor). Pods can talk with each other using kubernetes services. 

One VM can run multiple pods. 

Master has 4 main parts
- Store etcs
- Controller Manager
- Scheduler
- API Server

We use kubectl to talk with API Server that interacts with those parts.

Each agent (node) has Kubelet (Small agent installed) that can talk with master node. 

## 3. Benefits and use cases
// Containers
- Accelerate developer onboarding
- Eliminate App Conflicts
- Environment Consistency
- Ship softer faster

// Kubernetess
- Orchestrate Containers
- Zero Down-time depolyments
- Self-healing
- Scaling
- End to end testing environment, performance testing
- Ensures application scales properly
- Secrets and configuration
- Build servers
- Canary, A/B testing
- Help DevOps

## 4. kubectl cmds
```bash
- kubectl version
- kubectl cluster-info # view cluster information
- kubectl get all # Retrieve info about kubernetes pods, deployments, services and more. Ie kubectl get pods.
- kubectl run [container-name] --image=[image-name]
- kubectl port-forward [pod-name] [ports] # forward a port to allow external access. Example kubectl port-forward [name-of-pod] 8080:80. The first is extarnal port, then comes internal port.
- kubectl expose ... # Expose a port for deployment/pod
- kucectl create # create a resource
- kubectl apply # Create or modify resource. `kubectl apply -f file.pod.yml --dry-run --validate=true` will just pretented the deploy and give an output. It works with folders: all yamls will be applied!.
# 4_deployments
- kubectl get deployments
- kubectl get deployment --show-labels # List all deployments and their labels.
- kubectl get deployment -l app=nginx # Get all Deployments with a specific label
- kubectl delete deployment [deployment-name] # Delete deployment and all associated Pods/Containers.
- kubectl scale deployment [deployment-name] --replicas=5 # Scale pods horizontally (or update yaml file and use kubectl apply).

```

## 5. Pods
- Smallest unit of the kubernetes object model
- Environment for containers
- Organize application "parts" into Pods 
- Pod has ip, memory, volumes, etc. shared across contaienrs.
- Scale horizontaly by adding more pods
- Pods live or die but never come back to life
- Pods don't span nodes

Example of nginx with forwarding the port
```kubectl run my-nginx --image=nginx:alpine
kubectl get all
kubectl port-forward my-nginx 8080:80
kubectl delete pod/my-nginx```

Pods modes:
* The "one-container-per-Pod" model is the most common Kubernetes use case; in this case, you can think of a Pod as a wrapper around a single container; Kubernetes manages Pods rather than managing the containers directly.
* A Pod can encapsulate an application composed of multiple co-located containers that are tightly coupled and need to share resources. These co-located containers form a single cohesive unit of service—for example, one container serving data stored in a shared volume to the public, while a separate sidecar container refreshes or updates those files. The Pod wraps these containers, storage resources, and an ephemeral network identity together as a single unit.


alias
sudo snap alias microk8s.kubectl kubectl
token:
eyJhbGciOiJSUzI1NiIsImtpZCI6IlBKLXlVVDRCa2Q4MUVtMXZvY1FvMUVwVXBRbUJJWU9KSC1zQUstbXBzMFUifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZWZhdWx0LXRva2VuLThod3ZzIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRlZmF1bHQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIxNjQ0MzRhMi0yMzU0LTRkMTgtYWQ5NS01NDIwZjJhYTgwYTUiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZGVmYXVsdCJ9.HnHNv7wo8RnYATFgcpsm681T_yfKUqobCmeY0JPtP9bmh82DYglRokCAkinp-S6zwyKZlP7GCJi5rkqWPe1AEYXZfqnfQgo8kYlhJf_MHnBEcPi00N0YbIpUGPTKIu0nBJwQk1zMDepUhnJ3fvwOpkJA79JWblB1Hx5f0VdZnflgypIYWP7Co4LCjfE_FHD1HMYSDPcsBHXywHxVt2F5iOOa99dWg0O_jtT1W5uG262pmgscPGrDPKEgk3veYfZ8HggYtSuMR7dAxA1mphykwMXFxYJ_-3Ql6xenuQn80eiJN7NRx46bIm-TPn6aPtsAJoimDhz79zKG8bn-R6mXpw