aws eks update-kubeconfig --name dev-my-app-eks --region us-east-1
kubectl apply -k E:\CD-repo\observability

argocd:
  helm repo update
  kubectl create ns argocd 
  helm upgrade -install argocd argo/argo-cd -n argocd -f E:\CD-repo\Argocd\helm-values\argocd-values-9.4.0.yaml
  kubectl apply -f E:\CD-repo\Argocd\HTTProute


autoscaler:
helm upgrade -i autoscaler autoscaler/cluster-autoscaler -n kube-system  -f E:\CD-repo\addons\cluster-autoscaler\cluster-autoscaler-values.yaml
istio:
istioctl install --set profile=default -y

load balancer controller: 1.(Layer7) 2.(Layer4)
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.3.0/standard-install.yaml

kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.3.0/experimental-install.yaml 

helm upgrade -i aws-load-balancer-controller eks/aws-load-balancer-controller -n  kube-system -f E:\CD-repo\addons\aws-load-balancer-controller\aws-load-balancer-controller-values.yaml

kubectl apply -f E:\CD-repo\gateway-api-manifests

otell:
  kubectl create ns observability
  helm upgrade -i opentelemetrycollector-daemonset open-telemetry/opentelemetry-collector -n observability -f E:\CD-repo\addons\opentelemetry-collector-daemonset\daemonset.yaml

  helm upgrade -i opentelemetrycollector-gateway open-telemetry/opentelemetry-collector -n otel -f E:\CD-repo\addons\opentelemetry-collector-gateway\gateway.yaml

external-dns:
kubectl create ns external-dns
helm upgrade --install external-dns external-dns/external-dns -n external-dns -f E:\CD-repo\addons\external-dns\external-dns-values.yaml

promtheus,grafana stack:
  kubectl create ns monitoring 
  helm upgrade -i kube-prometheus-stack prometheus-community/kube-prometheus-stack -f E:\CD-repo\addons\prometheus-kube-stack\prometheus-kube-stack-values.yaml  -n monitoring 

ECK stack + jaeger:
  kubectl create ns observability
  helm upgrade --install eck-operator elastic/eck-operator -n observability -f E:\CD-repo\addons\eck-operator\eck-operator-values.yaml
  helm upgrade --install eck-elasticsearch elastic/eck-elasticsearch -n observability -f E:\CD-repo\addons\eck-elasticsearch\eck-elasticsearch-values.yaml
  helm upgrade --install eck-kibana elastic/eck-kibana -n observability -f E:\CD-repo\addons\eck-kibana\eck-kibana-values.yaml

  helm upgrade --install jaeger jaegertracing/jaeger -n observability -f E:\CD-repo\addons\jaeger\jaeger.yaml

    helm upgrade --install eck-beats elastic/eck-beats -f E:\CD-repo\addons\eck-beats-0.19.1.yaml



  kubectl get secret es-ca -n observability -o yaml \
| sed 's/namespa -n observability/namespace: observability/' \
| kubectl apply -n observability -f -


