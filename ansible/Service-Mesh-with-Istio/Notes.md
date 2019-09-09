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

Ingress <-> pod1[svc1,*proxy] <-> 