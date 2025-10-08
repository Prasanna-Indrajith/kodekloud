# Troubleshoot Deployment issues in Kubernetes

## Question

Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.

The deployment name is `redis-deployment`. The pods are not in running state right now, so please look into the issue and fix the same.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Answer

- Check the deployment is down or not

```bash
kubectl get deployments
```

- See what is going on `redis-deployment`

```bash
kubectl describe deployments.apps redis-deployment
```

in here I noticed two errors.

1. `Image: redis:alpin`❌ `Image: redis:alpine` ✅
2. `Name: redis-cofig` ❌ `Name: redis-config` ✅

Also there is one more issue but, I don't it helps or not to crash of the deployment. Which is

```yaml
resources:
  requests:
    cpu: 100m # Reduced from 300m
```

CPU request of 300m might be too high for the environment.

- Edit the deployment configuration

```bash
kubectl edit deployment redis-deployment
```

Make the changes accordingly

- Check the staus of Deployment and POD

```bash
kubectl get deployments

kubectl get pods
```

## Brief Description About the Environment

**Redis?**

Redis is an in-memory key-value database used primarily for caching and improving application performance. It allows for fast data access and can also function as a message broker, making it suitable for various applications like real-time analytics and session storage
