# Service Mesh with Istio

What can Istio do? It controls traffic between pods that does rate limiting, firewall rules, monitoring, logging and also authentication/authorization and encryption within kubernetes.

## Istio components

kubectl get pods -n istio-system (this will shows you all the pods runing)

Envoy(proxy)

* Dynamic service discovery
* Load balancing
* TLS termination
* Health checks
* Staged rollouts
* Fault injection

Pilot(control plane api)

* Service discovery
* Intelligent routing
* Resiliency

Citadel(control plane api)

* User authentication
* Credential management
* Certifcate management
* Traffic encryption

Mixer(control plane api)

* Access control
* Usage policies
* Telemetry data

## How Istio does its job

Ingress <-encrypted-> pod1[svc1,*proxy(sidecar)] <-encrypted-> pod2[svc2,*proxy(sidecar)] <-encrypted-> Egress
proxy(Envoy) is injected as a sidecar and shares the same network namespace with the containers in the same pod. Hence, all the iptables/firewalls

proxy will talk to Pilot/Citadel/Mixer for authorizing/encryption/routing decisions

kubectl get endpoints (shows the endpoints)


## Service with docker

add user to the docker group and install docker-compose

preconfigure kubectl for pilot
```
kubectl config set-context istio --cluster=istio
kubectl config set-cluster istio --server=http://localhost:8080
kubectl config use-context istio
```

create a DOCKER_GATEWAY env variable
export DOCKER_GATEWAY=172.28.0.1:

Bring up istio's control plane
docker-compose -f install/consul/istio.yaml up -d

Change bookinfo.yaml port #
sed -i 's/9081/30080/' ./istio-1.0.6/samples/bookinfo/platform/consul/bookinfo.yaml

Bring up the app
docker-compose -f ./istio-1.0.6/samples/bookinfo/platform/consul/bookinfo.yaml up -d

Bring up the sidecar
docker-compose -f ./istio-1.0.6/samples/bookinfo/platform/consul/bookinfo.sidecars.yaml up -d

## Istio with kubernetes

download the istio release and untar it
make sure istioctl is installed or use the one provided in the download

navigate to ISTIO_ROOT/install/kubernetes
modify istio-demo.yaml
change Service type from LoadBalancer to NodePort and http2 port to 30080 

run 
```console
kubectl apply -f ./istio-demo.yaml
```
watch
kubectl get pods -n istio-system 
until all pods are running without error

Deploy booinfo app with sidecar injection
kubectl apply -f <(istioctl kube-inject -f ISTIO_ROOT/samples/bookinfo/platform/kube/bookinfo.yaml)

kubectl get pods will show you some bookinfo pods in the default namespace

Add a gateway to the app
kubectl apply -f ISTIO_ROOT/samples/bookinfo/networking/bookinfo-gateway.yaml


