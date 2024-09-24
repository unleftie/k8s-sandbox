# k8s-sandbox

## Dependencies

- [Docker](https://docs.docker.com/get-docker/)
- [k3d](https://k3d.io/)

## 1. Prepare k3d environment

```bash
k3d cluster create
k3d node list
```

## 2. [Set up Dashboard UI for k3d](https://istio.io/latest/docs/setup/platform-setup/k3d/#set-up-dashboard-ui-for-k3d)

## 3. Prepare kube-prometheus-stack

```bash
k create namespace monitoring

k apply -n monitoring -f monitoring/prometheus-file-sd-targets.yml
helm install -n monitoring -f monitoring/blackbox.yml blackbox prometheus-community/prometheus-blackbox-exporter

k apply -n monitoring -f monitoring/alertmanager-secrets.yml
helm install -n monitoring -f monitoring/prometheus-stack.yml prometheus prometheus-community/kube-prometheus-stack

k apply -n monitoring -f monitoring/prometheus-external-alerting-rules-crd.yml
k apply -n monitoring -f monitoring/alertmanager-config-crd.yml
```
