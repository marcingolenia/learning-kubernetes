https://github.com/DanWahlin/CodeWithDanDockerServices

## Veiw the logs for a Pod's container
kubectl logs [pod-name]

## Steam logs (same for docker - docker logs -f)
kubectl logs -f [pod-name]

## View the logs for a specific container within a Pod
kubectl logs [pod-name] -c [container-name]

also see [1_theory.md](./1_theory.md). to get or describe por.