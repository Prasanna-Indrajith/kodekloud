# Set resource limit in Kubernetes pods

## Question

The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:

Create a pod named `httpd-pod` with a container named `httpd-container`. Use the `httpd` image with the `latest` tag (specify as `httpd:latest`). Set the following resource limits:

Requests: Memory: 15Mi, CPU: 100m
Limits: Memory: 20Mi, CPU: 100m

Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

## Answer

- Create file called `limied-pod.yml`

```yml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"
```

- run the deployment

```bash
kubectl apply -f limited-pod.yml
```

## Additional Details

Additional commands with `deployment`

Check the status of `pods`

```bash
kubectl get pods

# Check fot our requested and limited resources
kubectl describe pods httpd-pod
```

## Brief Description About the Environment

**Resource Management for Pods and Containers**

When you specify a Pod, you can optionally specify how much of each resource a container needs. The most common resources to specify are CPU and memory (RAM); there are others.

When you specify the resource request for containers in a Pod, the kube-scheduler uses this information to decide which node to place the Pod on. When you specify a resource limit for a container, the kubelet enforces those limits so that the running container is not allowed to use more of that resource than the limit you set. The kubelet also reserves at least the request amount of that system resource specifically for that container to use.
