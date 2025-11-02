# Deploy MySQL on Kubernetes

## Question

A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:

1.) Create a PersistentVolume mysql-pv, its capacity should be 250Mi, set other parameters as per your preference.

2.) Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as mysql-pv-claim and request a 250Mi of storage. Set other parameters as per your preference.

3.) Create a deployment named mysql-deployment, use any mysql image as per your preference. Mount the PersistentVolume at mount path /var/lib/mysql.

4.) Create a NodePort type service named mysql and set nodePort to 30007.

5.) Create a secret named mysql-root-pass having a key pair value, where key is password and its value is YUIidhb667, create another secret named mysql-user-pass having some key pair values, where frist key is username and its value is kodekloud_gem, second key is password and value is GyQkFRVNr3, create one more secret named mysql-db-url, key name is database and value is kodekloud_db1

6.) Define some Environment variables within the container:

a) name: MYSQL_ROOT_PASSWORD, should pick value from secretKeyRef name: mysql-root-pass and key: password

b) name: MYSQL_DATABASE, should pick value from secretKeyRef name: mysql-db-url and key: database

c) name: MYSQL_USER, should pick value from secretKeyRef name: mysql-user-pass key key: username

d) name: MYSQL_PASSWORD, should pick value from secretKeyRef name: mysql-user-pass and key: password

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Answer

### 1. Create Kubernetes Secrets

Create required secrets

```bash
# Create the secret for the root password
kubectl create secret generic mysql-root-pass --from-literal=password='YUIidhb667'

# Create the secret for the application user and password
kubectl create secret generic mysql-user-pass --from-literal=username='kodekloud_gem' --from-literal=password='GyQkFRVNr3'

# Create the secret for the database name
kubectl create secret generic mysql-db-url --from-literal=database='kodekloud_db1'
```

---

### 2. Configuration File (`mysql-deployment-setup.yaml`)

Create the following single YAML manifest file to define the remaining resources.

```yaml
# 1. PersistentVolume Definition (mysql-pv)
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi # Capacity must be 250Mi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce # ReadWriteOnce is a common access mode for a single-pod MySQL instance
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: "/mnt/data/mysql-data" # Path on the host node for data persistence
---
# 2. PersistentVolumeClaim Definition (mysql-pv-claim)
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi # Requesting 250Mi to match the PV
  storageClassName: manual
---
# 3. MySQL Deployment Definition (mysql-deployment)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql-server
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql-server
  strategy:
    type: Recreate # Use Recreate to safely handle the single volume mount
  template:
    metadata:
      labels:
        app: mysql-server
    spec:
      volumes:
        - name: mysql-persistent-storage
          persistentVolumeClaim:
            claimName: mysql-pv-claim
      containers:
        - name: mysql-container
          image: mysql:5.7 # Preferred MySQL image
          ports:
            - containerPort: 3306

          # 6. Define Environment Variables from Secrets
          env:
            # a) MYSQL_ROOT_PASSWORD
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-root-pass
                  key: password
            # b) MYSQL_DATABASE
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: mysql-db-url
                  key: database
            # c) MYSQL_USER
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: username
            # d) MYSQL_PASSWORD
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: password

          # 3. Mount the Persistent Volume at /var/lib/mysql
          volumeMounts:
            - name: mysql-persistent-storage
              mountPath: /var/lib/mysql
---
# 4. MySQL Service Definition (mysql)
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort # Service type is NodePort
  ports:
    - port: 3306
      targetPort: 3306
      nodePort: 30007 # Set NodePort to 30007
  selector:
    app: mysql-server
```

### 3. Apply the YAML file and verify

```bash
kubectl apply -f mysql-deployment-setup.yaml

kubectl get pv,pvc,deploy,svc,pod -l app=mysql-server
```

---

## 📘 Brief Description About the Environment

### PersistentVolume (PV) and PersistentVolumeClaim (PVC)

- **PV** is a cluster resource provisioned by an administrator (or dynamically) that exists independently of the Pod's lifecycle, ensuring data is persistent.
- **PVC** is a request for storage by a user/Pod. Kubernetes binds a PVC (e.g., `mysql-pv-claim` requesting `250Mi`) to a suitable PV (e.g., `mysql-pv` offering `250Mi`), allowing the Deployment to consume storage without knowing the storage implementation details.

### Service (NodePort)

- A **NodePort** Service provides a stable endpoint to access the MySQL Deployment from outside the cluster. It exposes the Service on a static port (in this case, **30007**) on every Node's IP address. Traffic received on this NodePort is forwarded to the Pod's port (3306).

### Secrets

- **Secrets** are used to store sensitive information (like the root and user passwords) securely in Kubernetes. By referencing these Secrets in the Deployment's **`env`** section using **`secretKeyRef`**, the application's environment variables are set without hardcoding credentials in the manifest file.
