# Revert Deployment to Previous Version in Kubernetes

## Question

## Answer

- Gather information we need to `rollout` update

  1. deployment name
  2. current revision (eg: revision=2)

- Check the `history` of update (`set`)

```bash
kubectl rollout history deployments/nginx-deployment
```

- Roll back to previous version

```bash
kubectl rollout undo deployments/nginx-deployment --to-revision=1

or

kubectl rollout undo deployments/nginx-deployment
```

- Check the `history` after roll back

```bash
kubectl rollout history deployments/nginx-deployment
```
