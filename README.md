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
k create namespace argo-cd

helm repo add argo https://argoproj.github.io/argo-helm

helm install -n argo-cd -f argo-cd/values.yml argo-cd argo/argo-cd
```

## 4. Prepare kube-prometheus-stack

```bash
k create namespace monitoring

k apply -n monitoring -f monitoring/prometheus-file-sd-targets.yml
helm install -n monitoring -f monitoring/blackbox.yml blackbox prometheus-community/prometheus-blackbox-exporter

k apply -n monitoring -f monitoring/alertmanager-secrets.yml
helm install -n monitoring -f monitoring/prometheus-stack.yml prometheus prometheus-community/kube-prometheus-stack

k apply -n monitoring -f monitoring/prometheus-external-alerting-rules-crd.yml
k apply -n monitoring -f monitoring/alertmanager-config-crd.yml
```
