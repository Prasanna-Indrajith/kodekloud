# Manage Secrets in Kubernetes

## Question

The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

We already have a secret key file `media.txt` under `/opt` location on jump host. Create a `generic secret` named `media`, it should contain the password/license-number present in `media.txt` file.

Also create a `pod` named `secret-xfusion`.

Configure pod's `spec` as container name should be `secret-container-xfusion`, image should be `ubuntu` with `latest` tag (remember to mention the tag with image). Use `sleep` command for container so that it remains in running state. Consume the created secret and mount it under `/opt/demo` within the container.

To verify you can exec into the container `secret-container-xfusion`, to check the secret key under the mounted path `/opt/demo`. Before hitting the `Check` button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

## Answer

- Create the secret first using the file

```bash
kubectl create secret generic media --from-file=/opt/media.txt
```

- apply the pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-xfusion
spec:
  containers:
    - name: secret-container-xfusion
      image: ubuntu:latest
      command: ["/bin/bash", "-c", "sleep infinity"]
      volumeMounts:
        - name: media-secret
          mountPath: /opt/demo
          readOnly: true
  volumes:
    - name: media-secret
      secret:
        secretName: media
```

- Apply the secret and

```bash
kubectl apply -f pod.yaml
```

- Check the secretes

```bash
kubectl exec -it secret-xfusion -- ls -la /opt/demo/
kubectl exec -it secret-xfusion -- cat /opt/demo/media.txt
```

## Brief Description About the Environment

**Secrets in Kubernetes?**

Kubernetes Secrets are API objects designed to store and manage sensitive information such as passwords, API keys, OAuth tokens, SSH keys, and certificates securely within a cluster.
They enable the separation of sensitive data from application code, reducing the risk of exposure during deployment and enhancing security by allowing controlled access to authorized pods.
Secrets can be created using various methods, including kubectl commands, YAML configuration files with base64-encoded data, or tools like Kustomize.
Once created, secrets can be consumed by pods through volume mounts or environment variables, and they support specific built-in types like Opaque, kubernetes.io/service-account-token, kubernetes.io/dockerconfigjson, kubernetes.io/tls, and kubernetes.io/basic-auth to cater to different use cases.
