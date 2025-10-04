# Execute Rolling Updates in Kubernetes

## Question

## Answer

- This is the strcuture that we need to apply

```bash
kubectl set image deployments/<deployment-name> <container-name>=<applicaion>:<new-version>
```

- Get the information that we need to rolling the update
  1. container name
  2. deployment name
  3. current version

```bash
kubectl describe deployments
```

- Rolling update by running this

```bash
kubectl set image deployments/nginx-deployment nginx-container=nginx:1.18
```

- Check status afetr update

```bash
kubectl rollout status deployments/nginx-deployment
```

- Ensure Pods are running correctly after update

```bash
kubectl get pods
```

## Brief Description About the Environment

**Rolling updates**

Rolling updates allow Deployments' update to take place with zero downtime by incrementally updating Pods instances with new ones.
