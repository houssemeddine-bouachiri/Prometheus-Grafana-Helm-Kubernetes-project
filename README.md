
# Prometheus, Grafana, Helm, and Kubernetes Project

![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?logo=prometheus&logoColor=white&style=flat)
![Grafana](https://img.shields.io/badge/Grafana-F46800?logo=grafana&logoColor=white&style=flat)
![Helm](https://img.shields.io/badge/Helm-0F1689?logo=helm&logoColor=white&style=flat)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white&style=flat)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white&style=flat)
![License](https://img.shields.io/badge/License-MIT-yellow)

This project provides a complete **Kubernetes**-based environment for deploying and monitoring applications using **Helm**, **Prometheus** and **Grafana**.

The infrastructure combines:

- Community Helm charts from Artifact Hub
- Custom local Helm charts
- Kubernetes manifests and configurations
- Monitoring and observability with prometheus and grafana

The objective is to automate application deployment while maintaining scalability, modularity, and operational visibility.

<https://kubernetes.io/docs/home/>

<https://helm.sh/docs/>

<https://prometheus.io/docs/introduction/overview/>

<https://grafana.com/docs/>

---

## Install Prometheus and Grafana from Artifact Hub

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo ls
helm search repo prometheus-community
helm install my-prometheus prometheus-community/prometheus --version 29.6.0
helm ls --all-namespaces
helm status my-prometheus
export POD_NAME=$(kubectl get pods --namespace default -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace default port-forward $POD_NAME 9090
```

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo ls
helm search repo bitnami
helm install my-grafana bitnami/grafana --version 12.1.8
helm ls --all-namespaces
helm status my-grafana
export POD_NAME=$(kubectl get pods --namespace default -l "app=grafana,component=server" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace default port-forward $POD_NAME 3000
```

## More Helm commands

```
helm install my-prometheus-dev prometheus-community/prometheus --version 29.6.0 --namespace dev
helm ls --all-namespaces
helm uninstall my-prometheus-dev --keep-history
helm ls --all-namespaces -a
helm upgrade my-prometheus-dev prometheus-community/prometheus --version 27.6.0 --namespace dev
helm history my-prometheus-dev -n dev 
helm rollback my-prometheus-dev 1 -n dev
helm show values prometheus-community/prometheus
```

## Create the helmchart

```
helm create webapp1
ls -ltra
```

## Install the helmchart

```
helm install mywebapp-release webapp1/ --values webapp1/values.yaml
kubectl get all
```

## Upgrade after templating

```
helm upgrade mywebapp-release webapp1/ --values webapp1/values.yaml
helm ls
kubectl get all
```

## Accessing it

```
minikube tunnel
```

## Create dev/prod

```
kubectl create namespace dev
kubectl create namespace prod
helm install mywebapp-release-dev webapp1/ --values webapp1/values.yaml -f webapp1/values-dev.yaml -n dev
helm install mywebapp-release-prod webapp1/ --values webapp1/values.yaml -f webapp1/values-prod.yaml -n prod
helm ls --all-namespaces
kubectl get all -n dev
kubectl get all -n prod
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

**Star this repository if it helped you!**
