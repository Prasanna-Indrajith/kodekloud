# Deploy Grafana on Kubernetes Cluster

## Question

The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.

1. Create a deployment named `grafana-deployment-nautilus` using any grafana image for Grafana app. Set other parameters as per your choice.

2. Create `NodePort` type `service` with nodePort `32000` to expose the app.

`You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.`

Note: The kubectl on jump_host has been configured to work with kubernetes cluster.

## Answer

- Create grafana-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-nautilus
spec:
  replicas: 1
  selector:
    matchLables:
      app: grafana
  template:
    metadata:
      lables:
        app: grafana
    spec:
      containers:
        - name: grafana-container
          image: grafana/grafana:latest
          ports:
            - containerPort: 3000
          env:
            - name: GF_SECURITY_ADMIN_PASSWORD
              value: "admin"
            - name: GF_SECURITY_ADMIN_USER
              value: "admin"
---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
```

- Apply the deployment

```bash
kubectl apply -f grafana-deployment.yaml
```

- Verify deployment

```bash
# Check deployment status
kubectl get deployments

# Check pod status
kubectl get pods

# Check service
kubectl get services
```

## Brief Description About the Environment

**Grafana?**

**Grafana** is a multi-platform open source analytics and interactive visualization web application. It can produce charts, graphs, and alerts for the web when connected to supported data sources
