# Utilities for operating Prow

update-config:
	kubectl create configmap config --from-file=config.yaml=config.yaml --dry-run=client -o yaml | kubectl apply -f -

update-plugins:
	kubectl create configmap plugins --from-file=plugins.yaml=plugins.yaml --dry-run=client -o yaml | kubectl apply -f -

update-label-config:
	kubectl create configmap label-config --from-file=labels.yaml=labels.yaml --dry-run=client -o yaml | kubectl apply -f -

deploy-prow: update-config update-plugins update-label-config
	kubectl apply --server-side=true -f ./cluster/prowjob-crd/prowjob_customresourcedefinition.yaml
	kubectl apply -f ./cluster/

deploy-openebs:
	helm repo add openebs https://openebs.github.io/charts
	helm repo update
	helm install openebs --namespace openebs openebs/openebs --create-namespace
	kubectl patch storageclass openebs-hostpath -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'

deploy-ingress-controller:
	helm upgrade --install ingress-nginx ingress-nginx \
      --repo https://kubernetes.github.io/ingress-nginx \
      --namespace ingress-nginx --create-namespace

deploy-certmanager:
	helm repo add jetstack https://charts.jetstack.io
	helm repo update
	kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.crds.yaml
	helm install \
	  cert-manager jetstack/cert-manager \
	  --namespace cert-manager \
	  --create-namespace \
	  --version v1.8.0

deploy-metallb:
	kubectl get configmap kube-proxy -n kube-system -o yaml | \
    sed -e "s/strictARP: false/strictARP: true/" | \
    kubectl apply -f - -n kube-system
	helm repo add metallb https://metallb.github.io/metallb
	helm repo update
	helm upgrade --install metallb metallb/metallb -n metallb-system --create-namespace
