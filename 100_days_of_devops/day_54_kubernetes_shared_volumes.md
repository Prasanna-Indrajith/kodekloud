# Kubernetes Shared Volumes

## Question

We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.

Create a pod named `volume-share-devops`.

For the first container, use **image** `fedora` with `latest` **tag** only and remember to mention the tag i.e `fedora:latest`, container should be **named** as `volume-container-devops-1`, and **run** a `sleep` command for it so that it remains in running state. **Volume** `volume-share` should be **mounted at path** `/tmp/ecommerce`.

For the second container, use **image** `fedora` with the `latest` **tag** only and remember to mention the tag i.e `fedora:latest`, container should be **named** as `volume-container-devops-2`, and again **run** a `sleep` command for it so that it remains in running state. **Volume** `volume-share` should be **mounted at path** `/tmp/demo`.

**Volume name** should be `volume-share` of **type** `emptyDir`.

After creating the pod, `exec` into the first container i.e `volume-container-devops-1`, and just for testing **create a file** `ecommerce.txt` with any content under the mounted path of first container i.e `/tmp/ecommerce`.

The file `ecommerce.txt` should be present under the mounted path `/tmp/demo` on the second container `volume-container-devops-2` as well, since they are using a shared volume.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Answer

- Create a `pod.yml` file

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  volumes:
    - name: volume-share
      emptyDir: {}
  containers:
    - name: volume-container-devops-1
      image: fedora:latest
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/ecommerce
      command: ["/bin/sh", "-c"]
      args: ["sleep 3600"]

    - name: volume-container-devops-2
      image: fedora:latest
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/demo
      command: ["/bin/sh", "-c"]
      args: ["sleep 3600"]
```

- Create the pod with 2 containers inside
- Check status

```bash
kubectl apply -f pod.yml

kubectl get pods
```

- Create a file inside `volume-share-devops` pod `volume-container-devops-1` container `/tmp/ecommerce` directory

```bash
kubectl exec volume-share-devops -c volume-container-devops-1 -- sh -c "echo 'Test echoo' > /tmp/ecommerce/ecommerce.txt"
```

- Check the file is exist in `volume-container-devops-2`

```bash
kubectl exec volume-share-devops -c volume-container-devops-2 -- sh -c "cat/tmp/demo/ecommerce.txt"
```

## Additional Details

When `exec` command inside kubectl

```bash
kubectl exec <pod-name> -c <container-name> -- sh -c "cat/tmp/demo/ecommerce.txt"
```

- `sh -c` - shell command

## Brief Description About the Environment

**Shared volumes**

A shared volume in Kubernetes allows multiple containers within the same Pod to access a common storage area, enabling data sharing and coordination.

- Defined in `.spec.volumes` and mounted into containers via `.spec.containers[*].volumeMounts`.
- Common types: `emptyDir` (ephemeral), `persistentVolumeClaim` (persistent), `configMap`, `secret`.
- Changes made by one container are visible to others.
- Ensures data persistence across container restarts within the Pod.
