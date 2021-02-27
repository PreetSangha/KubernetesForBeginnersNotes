# What is Kubernetes

- An open source container orchestration tool from google

## what problems does it solve

- managing many containers (100s, 1000s etc) is complex
- high availability
- scalability
- disaster recovery

## main components

### pod

- an abstraction over container
- usually one app per pod
- gets onw ip address for -> pod communication
- gets new ip address on pod restart

### service

- communications between pods
- static address that can attached to pod
- has own lifetime to pod
- can be internal or external (to allow communication outside the cluster)
- external addresses are pod namespaces so not always ergonomic. To help you can use an ingress
- effectively load balances between multiple pods in the same service

### ingress

- route external information into a cluster 
- kind of service that is external 
- has external address that is public to the world and redirects through to the correct internal service

### configMap

- stores external configuration information used by containers
- pod get the information automatically
- plaintext so insecure

### secret

- like configmap but stores secrets
- **need to enable security**

### volumes

- persistence for pods
- attaches physical storage to the pod
- storage could local or remote
- k8s doesn't manage data persistence as that's your apps' job

### node

- basic unit of compute (server/machine/vm/etc)
- runs pods
- allows the pods to have resiliency by allowing them to be distributed

## pod blueprints

- yaml document to describe deployments and statefulsets

### deployment

- blueprint for pods in terms of resiliency 
- but not ideal for things that have state - like dbs

### statefulset

- used to ensure that pods have state (like dbs)
- hard to use
- probably better use move DBs out of the dbs