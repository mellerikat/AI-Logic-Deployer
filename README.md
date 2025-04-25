## prerequisite
> **Note**  
> You can skip this step if `istio`, `prometheus` and `kiali` are already installed

Install istio, prometheus, kiali
```bash
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
helm install istio-base istio/base -n istio-system --set defaultRevision=default --create-namespace
helm upgrade istio-base istio/base -n istio-system --set prometheus.enabled=true
helm install istiod istio/istiod -n istio-system --wait
helm install istio-ingressgateway istio/gateway -n istio-system
helm install --namespace istio-system --set auth.strategy="anonymous" --repo https://kiali.org/helm-charts kiali-server kiali-server
helm install prometheus prometheus-community/prometheus -n istio-system
kubectl patch storageclass gp2 -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

## installation

Add the required repositories.
```bash
helm repo add mellerikat-ald https://mellerikat.github.io/AI-Logic-Deployer
```
Verify that the repositories have been added correctly.
```bash
helm repo ls

NAME                            URL
mellerikat-ald               https://mellerikat.github.io/AI-Logic-Deployer
```

Install AI Logic Deployer.
```bash
helm install mk-ald mellerikat-ald/ald-common-resouce --values {values-file.yaml} -n mk-common
helm install mk-ald mellerikat-ald/ald-server --values {values-file.yaml} -n mk-ald
```