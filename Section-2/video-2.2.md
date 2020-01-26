# Path and Header based Routing

## Prerequisites

This video assumes that you have running Istio installation on your Kubernetes cluster.

Optionally, create a dedicated namespace for this showcase and label it appropriately for the sidecar injector webhook to work. Or simply use the default namespace.

```
$ kubectl label namespace default istio-injection=enabled
```

## Running

```
$ kubectl get svc istio-ingressgateway -n istio-system
$ export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')

# deploy sample application
$ kubectl apply -f kubernetes/hello-istio.yaml

# create ingress gateway and route traffic to microservices
$ kubectl apply -f kubernetes/hello-istio-gateway.yaml
$ kubectl apply -f kubernetes/hello-istio-virtual-service.yaml

# apply the version subsets as destinations
$ kubectl apply -f kubernetes/hello-istio-destination.yaml

# apply path based routing
$ kubectl apply -f kubernetes/hello-istio-uri-match.yaml

# apply header based routing
$ kubectl apply -f hello-istio-user-agent.yaml
$ http get $INGRESS_HOST/api/hello User-Agent:Chrome

$ kubectl apply -f hello-istio-user-cookie.yaml
$ http get $INGRESS_HOST/api/hello Cookie:user=packtpub
```