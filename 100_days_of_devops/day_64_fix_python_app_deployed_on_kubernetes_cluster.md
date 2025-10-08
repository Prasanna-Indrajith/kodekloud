# Fix Python App Deployed on Kubernetes Cluster

## Question

One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

The deployment name is `python-deployment-datacenter`, its using `poroko/flask-demo-appimage`. The deployment and service of this app is already deployed.

`nodePort` should be `32345` and `targetPort` should be python flask app's default port.`5000`

Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.

## Answer

- Check the current status of all the components

```bash
kubectl get deployments,pods,services
```

- In `pods` section says something replated to problem in pulling image
- To check further go into `pod`

```bash
kubectl describe pod <pod-name>
```

This also shows the `image name is wrong` content. The correct image name is given as `poroko/flask-demo-app`.

- Let's check deployment yml file to address this image issue

```bash
kubectl edit deployment python-deployment-datacenter
```

- In config yml file I did see image issue `poroko/flask-app-demo` ❌ `poroko/flask-demo-app` ✅
- Change the image into correct one. After like 10s check the `deployment` and `pod` status

```yaml
image: poroko/flask-demo-app

# check again status
kubectl get deployments,pods
```

Here you will see the deployment and pod is up and running

- Now check the `Services` for `nodePort` and `targetPort`
- `<service-name>` can get from `kubectl get services`

```bash
kubectl edit service <service-name>
```

- In this yml structure we can find `nodePort: 32345` and `targetPort:8080`
- python flask app's default port is port `5000`

```yaml
nodePort: 32345
targetPort: 5000 # change to this
```
