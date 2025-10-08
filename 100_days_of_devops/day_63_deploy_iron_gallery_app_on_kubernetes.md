# Deploy Iron Gallery App on Kubernetes

## Question

There is an iron gallery app that the Nautilus DevOps team was developing. They have recently customized the app and are going to deploy the same on the Kubernetes cluster. Below you can find more details:

Create a namespace iron-namespace-xfusion

Create a deployment iron-gallery-deployment-xfusion for iron gallery under the same namespace you created.

:- Labels run should be iron-gallery.

:- Replicas count should be 1.

:- Selector's matchLabels run should be iron-gallery.

:- Template labels run should be iron-gallery under metadata.

:- The container should be named as iron-gallery-container-xfusion, use kodekloud/irongallery:2.0 image ( use exact image name / tag ).

:- Resources limits for memory should be 100Mi and for CPU should be 50m.

:- First volumeMount name should be config, its mountPath should be /usr/share/nginx/html/data.

:- Second volumeMount name should be images, its mountPath should be /usr/share/nginx/html/uploads.

:- First volume name should be config and give it emptyDir and second volume name should be images, also give it emptyDir.

Create a deployment iron-db-deployment-xfusion for iron db under the same namespace.

:- Labels db should be mariadb.

:- Replicas count should be 1.

:- Selector's matchLabels db should be mariadb.

:- Template labels db should be mariadb under metadata.

:- The container name should be iron-db-container-xfusion, use kodekloud/irondb:2.0 image ( use exact image name / tag ).

:- Define environment, set MYSQL_DATABASE its value should be database_blog, set MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD value should be with some complex passwords for DB connections, and MYSQL_USER value should be any custom user ( except root ).

:- Volume mount name should be db and its mountPath should be /var/lib/mysql. Volume name should be db and give it an emptyDir.

Create a service for iron db which should be named iron-db-service-xfusion under the same namespace. Configure spec as selector's db should be mariadb. Protocol should be TCP, port and targetPort should be 3306 and its type should be ClusterIP.

Create a service for iron gallery which should be named iron-gallery-service-xfusion under the same namespace. Configure spec as selector's run should be iron-gallery. Protocol should be TCP, port and targetPort should be 80, nodePort should be 32678 and its type should be NodePort.

Note:

We don't need to make connection b/w database and front-end now, if the installation page is coming up it should be enough for now.

The kubectl on jump_host has been configured to work with the kubernetes cluster.

## Answer

- Create `iron-gallery-setup.yaml`

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-xfusion
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-xfusion
  namespace: iron-namespace-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
        - name: iron-gallery-container-nautilus
          image: kodekloud/irongallery:2.0
          resources:
            limits:
              memory: "100Mi"
              cpu: "50m"
          volumeMounts:
            - name: config
              mountPath: /usr/share/nginx/html/data
            - name: images
              mountPath: /usr/share/nginx/html/uploads
          volumes:
            - name: config
              emptyDir: {}
            - name: images
              emptyDir: {}
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-nautilus
  namespace: iron-namespace-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
        - name: iron-db-container-nautilus
          image: kodekloud/irondb:2.0
          env:
            - name: MYSQL_DATABASE
              value: "database_apache"
            - name: MYSQL_ROOT_PASSWORD
              value: "RootPass123!"
            - name: MYSQL_PASSWORD
              value: "RootPass123!"
            - name: MYSQL_USER
              value: "gallery_user"
          volumeMounts:
            - name: db
              mountPath: /var/lib/mysql
      volumes:
        - name: db
          emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-nautilus
  namespace: iron-namespace-xfusion
spec:
  selector:
    db: mariadb
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
  type: ClusterIP
---
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-nautilus
  namespace: iron-namespace-nautilus
spec:
  selector:
    run: iron-gallery
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 32678
  type: NodePort
```

- To Apply This Configuration

```bash
kubectl apply -f iron-gallery-setup.yaml
```

## Additional Details

Verification Commands

```bash
# Check namespace
kubectl get namespaces

# Check deployments
kubectl get deployments -n iron-namespace-nautilus

# Check pods
kubectl get pods -n iron-namespace-nautilus

# Check services
kubectl get services -n iron-namespace-nautilus

# Check service details
kubectl describe service iron-gallery-service-nautilus -n iron-namespace-nautilus
```

## Brief Description About the Environment

**namespace**

A Kubernetes namespace is a logical division within a cluster that provides a way to partition resources into isolated groups, enabling multiple teams, projects, or environments to coexist within the same cluster without interference.It acts as a virtual cluster inside the main cluster, allowing resources like pods, services, and deployments to be organized and managed independently. Namespaces provide a scope for resource names, ensuring uniqueness within a namespace but not across namespaces, which helps prevent naming conflicts.
