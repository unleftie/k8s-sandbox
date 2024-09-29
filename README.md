# k8s-sandbox

## Dependencies

- [Docker](https://docs.docker.com/get-docker/)
- [k3d](https://k3d.io/)

## 1. Prepare k3d environment

```bash
export INTERNAL_IP="10.10.10.10"
k3d cluster create remote --api-port 6443 --k3s-arg "--tls-san=$INTERNAL_IP"@server:\*
k3d node list
k3d kubeconfig get remote
```

## 2. [Set up Dashboard UI for k3d](https://istio.io/latest/docs/setup/platform-setup/k3d/#set-up-dashboard-ui-for-k3d)

## 3. Prepare argo-cd

```bash
k create namespace argocd
helm install -n argocd argocd argo/argo-cd
```

## 4. Prepare monitoring stack

```bash
k create namespace monitoring
k apply -n monitoring -f monitoring/kube-prometheus-stack/alertmanager-secrets.yml
k apply -n argocd -f monitoring/root.yml
```
