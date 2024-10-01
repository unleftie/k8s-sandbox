# k8s-sandbox

## Dependencies

- [Docker](https://docs.docker.com/get-docker/)
- [k3d](https://k3d.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) (k is an alias for kubectl)

## 1. Prepare k3d environment

```bash
export INTERNAL_IP="10.10.10.10"
k3d cluster create remote --api-port 6443 --k3s-arg "--tls-san=$INTERNAL_IP"@server:\*
k3d node list # check if the node is running
k3d kubeconfig get remote # get kubeconfig to access the cluster
```

## 2. Prepare Argo CD

```bash
k create namespace argocd
helm install -n argocd argocd argo/argo-cd
```

## 3. Prepare monitoring stack

```bash
k create namespace monitoring
k apply -n monitoring -f monitoring/kube-prometheus-stack/secrets/alertmanager-secrets.yml
k apply -n argocd -f root.yml
k apply -n argocd -f crds.yml
```
