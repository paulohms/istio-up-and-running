# istio-up-and-running

Pocs executed in reading the book Istio Up and Running


#### Cluster criation

```
kind create cluster --image kindest/node:v1.13.12
```

#### Kubernetes dashboard criation and get secret

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml

kubectl proxy

kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | awk '/default-token/ {print $1}') 

```

#### Download istio in specific version of book

```
curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.1.0 sh -

cd istio-1.1.0 

export PATH=$PWD/bin:$PATH

istioctl version
```

#### Install istio in kubernetes

```
for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done

kubectl apply -f install/kubernetes/istio-demo.yaml

```

#### After all pods are in ready state

```
istioctl proxy-status
```

#### Deployment bookinfo plataform and networking

```
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```

#### Get review product page endpoint

```
echo "http://$(kubectl get nodes -o template --template='{{range.items}}{{range.status.addresses}}{{if eq .type "InternalIP"}}{{.address}}{{end}}{{end}}{{end}}'):$(kubectl get svc istio-ingressgateway -n istio-system -o jsonpath='{.spec.ports[0].nodePort}')/productpage"
```

#### Deployment bookinfo application with Envoy sidecard

```
istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml | kubectl apply -f -
```