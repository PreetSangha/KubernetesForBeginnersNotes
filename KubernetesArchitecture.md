# Kubernetes Architecture

## worker nodes

- runs pods
- needs enough resources
- `container runtime` (e.g. docker)
- `kubelet` 
  - interfaces with container runtime to carry out actions
  - exposes an api
  - talks to the services to carry stuff out
- `kubeproxy`
  - ensures that pods communicate efficiently (e.g of the service you want is oen the same node then doesn't send it over the network)  

## master nodes

- need lower resources
- `Api Server`
  - you access this to control the cluster (eg. using the kubernetes cli)
  - start, stop pods etc.
- `Scheduler`
  - intelligence to ensure pods are run in the correct way in the right node.
- `controller manager`
  - detects pod state changes (started, failed etc.)
  - talks to scheduler to ensure the pods' desired states
- `etcd`
  - durable key value store for cluster operations
  - for instance what's the state of pod X?
  - doesn't store app data, it's cluster info only
