# Deploy Pods in Kubernetes Cluster

## Question

iThe Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:

Create a pod named pod-httpd using the httpd image with the latest tag. Ensure to specify the tag as httpd:latest.
Set the app label to httpd_app, and name the container as httpd-container.

Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

## Answer

- Create `pod.yml`
```yml
apiVersion: v1
kind: Pod
metadata:
  name: pod-httpd
  labels:
    app: httpd_app container:
  - name: httpd-container
    image: httpd:latest
```

- Now run it
```bash
kubectl apply -f pod.yml
```

## Additional Details

Additional commands with kubectl

Provide Description
```bash
# What are the created pods we have
kubectl get pods

# Full explanation about out pod
kubectl describe pod pod-httpd
```

## Brief Description About the Environment

**Why kubectl is good with .yml**

kubectl works well with .yml files because they provide a declarative, readable, and reusable way to define Kubernetes resources like Pods, Deployments, and Services.

Key Advantages:
Declarative Configuration: You specify the desired state (e.g., container name, image, ports), and Kubernetes ensures it’s achieved.
Version Control: YAML files can be stored in Git, enabling tracking of changes, rollbacks, and collaboration.
Consistency: Ensures identical deployments across environments (dev, staging, prod).
Complex Configurations: Easily define multi-container Pods, volumes, environment variables, and labels—hard to do with CLI commands.
Automation: Integrates smoothly with CI/CD pipelines.
While kubectl run can create simple Pods without YAML, YAML is the standard for production use due to its precision and scalability.
