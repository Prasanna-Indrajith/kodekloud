# Deploy Guest Book App on Kubernetes

## Question

The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details:

BACK-END TIER
Create a deployment named redis-master for Redis master.
a.) Replicas count should be 1.
b.) Container name should be master-redis-nautilus and it should use image redis.
c.) Request resources as CPU should be 100m and Memory should be 100Mi.
d.) Container port should be redis default port i.e 6379.
Create a service named redis-master for Redis master. Port and targetPort should be Redis default port i.e 6379.

Create another deployment named redis-slave for Redis slave.
a.) Replicas count should be 2.
b.) Container name should be slave-redis-nautilus and it should use gcr.io/google\_samples/gb-redisslave:v3 image.
c.) Requests resources as CPU should be 100m and Memory should be 100Mi.
d.) Define an environment variable named GET\_HOSTS\_FROM and its value should be dns.
e.) Container port should be Redis default port i.e 6379.
Create another service named redis-slave. It should use Redis default port i.e 6379.
FRONT END TIER
Create a deployment named frontend.
a.) Replicas count should be 3.
b.) Container name should be php-redis-nautilus and it should use gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff image.
c.) Request resources as CPU should be 100m and Memory should be 100Mi.
d.) Define an environment variable named as GET\_HOSTS\_FROM and its value should be dns.
e.) Container port should be 80.
Create a service named frontend. Its type should be NodePort, port should be 80 and its nodePort should be 30009.
Finally, you can check the guestbook app by clicking on App button.

You can use any labels as per your choice.
Note: The kubectl utility on jump\_host has been configured to work with the kubernetes cluster.

## Answer

  - Create `guestbook-app.yaml` with the following content:

<!-- end list -->

```yaml
---
# BACK-END TIER: Redis Master Deployment (1 Replica)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
  labels:
    app: guestbook
    tier: master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: guestbook
      tier: master
  template:
    metadata:
      labels:
        app: guestbook
        tier: master
    spec:
      containers:
      - name: master-redis-nautilus
        image: redis
        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"
        ports:
        - containerPort: 6379
---
# BACK-END TIER: Redis Master Service
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
    app: guestbook
    tier: master
  ports:
  - port: 6379
    targetPort: 6379
  type: ClusterIP
---
# BACK-END TIER: Redis Slave Deployment (2 Replicas)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
  labels:
    app: guestbook
    tier: slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: guestbook
      tier: slave
  template:
    metadata:
      labels:
        app: guestbook
        tier: slave
    spec:
      containers:
      - name: slave-redis-nautilus
        image: gcr.io/google_samples/gb-redisslave:v3
        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"
        ports:
        - containerPort: 6379
        env:
        - name: GET_HOSTS_FROM
          value: dns
---
# BACK-END TIER: Redis Slave Service
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  selector:
    app: guestbook
    tier: slave
  ports:
  - port: 6379
    targetPort: 6379
  type: ClusterIP
---
# FRONT-END TIER: Frontend Deployment (3 Replicas)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
      tier: frontend
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis-nautilus
        image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"
        ports:
        - containerPort: 80
        env:
        - name: GET_HOSTS_FROM
          value: dns
---
# FRONT-END TIER: Frontend Service (NodePort)
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  selector:
    app: guestbook
    tier: frontend
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30009
```

  - To Apply This Configuration:

<!-- end list -->

```bash
kubectl apply -f guestbook-app.yaml
```

## Additional Details

### Verification Commands

```bash
# Check the status of all created resources labeled 'guestbook'
kubectl get all -l app=guestbook

# View the log of the Redis Master pod to confirm it is running correctly
# Replace <REDIS-MASTER-POD-NAME> with the actual pod name
kubectl logs deployment/redis-master -f
```

### Brief Description About the Environment

The application uses **DNS Service Discovery** where the `GET_HOSTS_FROM=dns` environment variable allows the frontend (PHP) to communicate with the Redis Services (`redis-master`, `redis-slave`) using their service names instead of fixed IP addresses.

  * **Redis Master Deployment** (`redis-master`): Single Pod for write operations.
  * **Redis Slave Deployment** (`redis-slave`): Two Replicas for read operations, configured to sync data from the master.
  * **Frontend Deployment** (`frontend`): Three Replicas of the PHP web application.
  * **Frontend Service** (`frontend`): Exposed via **NodePort 30009** for external access to the application.