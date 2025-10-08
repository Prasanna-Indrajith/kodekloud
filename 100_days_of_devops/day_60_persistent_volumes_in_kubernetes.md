# Persistent Volumes in Kubernetes

## Question

The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:

Create a `PersistentVolume` named as `pv-nautilus`. Configure the `spec` as storage class should be `manual`, set capacity to `4Gi`, set access mode to `ReadWriteOnce`, volume type should be `hostPath` and set path to `/mnt/security` (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

Create a `PersistentVolumeClaim` named as `pvc-nautilus`. Configure the `spec` as storage class should be `manual`, request `2Gi` of the storage, set access mode to `ReadWriteOnce`.

Create a `pod` named as `pod-nautilus`, mount the persistent volume you created with claim name `pvc-nautilus` at document root of the web server, the container within the pod should be named as c`ontainer-nautilus` using image `nginx` with `latest` tag only (remember to mention the tag i.e `nginx:latest`).

Create a node port type service named `web-nautilus` using node port `30008` to expose the web server running within the pod.

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

## Answer

- Create `nautilus-setup.yaml`

```yaml
# PersistentVolume
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nautilus
spec:
  capacity:
    storage: 4Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  hostPath:
    path: /mnt/security
---
# PersistentVolumeClaim
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nautilus
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  resources:
    requests:
      storage: 2Gi
---
# Pod
apiVersion: v1
kind: Pod
metadata:
  name: pod-nautilus
  labels:
    app: nautilus-pod # Added label for service selector
spec:
  containers:
    - name: container-nautilus
      image: nginx:latest
      volumeMounts:
        - name: nautilus-storage
          mountPath: /usr/share/nginx/html # Document root for nginx
  volumes:
    - name: nautilus-storage
      persistentVolumeClaim:
        claimName: pvc-nautilus
---
# Service
apiVersion: v1
kind: Service
metadata:
  name: web-nautilus
spec:
  type: NodePort
  selector:
    app: nautilus-pod # Note: We need to add this label to the pod
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
```

- Apply the changes

```bash
kubectl apply -f nautilus-setup.yaml
```

- Veryfying

```bash
kubectl get pv
kubectl get pvc
kubectl get pods
kubectl get services
```

## Brief Description About the Environment

**PersistentVolume (PV)?**

What it is: A cluster-wide storage resource (like a physical disk)

Analogy: Think of it as a "storage device" available in the cluster

**PersistentVolumeClaim (PVC)?**

What it is: A "request" for storage by a pod

Analogy: Like requesting "I need 2GB of storage please"
