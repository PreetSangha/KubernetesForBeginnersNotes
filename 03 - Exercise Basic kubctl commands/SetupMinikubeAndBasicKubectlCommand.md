
https://gitlab.com/nanuchi/youtube-tutorial-series/-/blob/master/basic-kubectl-commands/cli-commands.md


# install prereqs

```

gsudo

// install hyperv

choco install docker-desktop

choco install minikube


```

# start minikube

```

➜  minikube start
😄  minikube v1.17.1 on Microsoft Windows 10 Enterprise 10.0.21301 Build 21301
🆕  Kubernetes 1.20.2 is now available. If you would like to upgrade, specify: --kubernetes-version=v1.20.2
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.19.4 preload ...
    > preloaded-images-k8s-v8-v1....: 487.14 MiB / 487.14 MiB  100.00% 39.16 Mi
🤷  docker "minikube" container is missing, will recreate.
🔥  Creating docker container (CPUs=2, Memory=7910MB) ...
🐳  Preparing Kubernetes v1.19.4 on Docker 19.03.13 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
~

➜  minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
timeToStop: Nonexistent


➜  kubectl version
Client Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.4", GitCommit:"e87da0bd6e03ec3fea7933c4b5263d151aafd07c", GitTreeState:"clean", BuildDate:"2021-02-18T16:12:00Z", GoVersion:"go1.15.8", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.4", GitCommit:"d360454c9bcd1634cf4cc52d1867af5491dc9c5f", GitTreeState:"clean", BuildDate:"2020-11-11T13:09:17Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"linux/amd64"}
~

➜  kubectl get nodes
NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   3m30s   v1.19.4

```

# creating a deployment 

```


➜  kubectl create deployment nginx-depl --image=nginx
deployment.apps/nginx-depl created
~
➜  kubectl get pods
NAME                          READY   STATUS              RESTARTS   AGE
nginx-depl-5c8bf76b5b-wwrm2   0/1     ContainerCreating   0          2s
~
➜  kubectl get replicaset
NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-5c8bf76b5b   1         1         0       9s
~
➜  kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-depl-5c8bf76b5b-wwrm2   1/1     Running   0          20s
~
```

# changing the default editor

```

~
➜  [System.Environment]::SetEnvironmentVariable('KUBE_EDITOR', 'code -n --wait', [System.EnvironmentVariableTarget]::User)
~
➜  refreshenv
Refreshing environment variables from the registry for powershell.exe. Please wait...
Finished
~

```

# editing the config to change the version of nginx 


```

➜  kubectl edit deployment nginx-depl
deployment.apps/nginx-depl edited
~
➜  kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
nginx-depl   1/1     1            1           11m
~
➜  kubectl get pods
NAME                          READY   STATUS        RESTARTS   AGE
nginx-depl-5c8bf76b5b-wwrm2   0/1     Terminating   0          11m
nginx-depl-7fc44fc5d4-ngbz5   1/1     Running       0          19s
~
➜  kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-depl-7fc44fc5d4-ngbz5   1/1     Running   0          56s
~
```

# looking at logs

```

➜  kubectl create deployment mongo-depl --image=mongo
deployment.apps/mongo-depl created
~
➜  kubectl get pods
NAME                          READY   STATUS              RESTARTS   AGE
mongo-depl-5fd6b7d4b4-p6jjk   0/1     ContainerCreating   0          5s
nginx-depl-7fc44fc5d4-ngbz5   1/1     Running             0          3m20s
~
➜  kubectl logs mongo-depl-5fd6b7d4b4-p6jjk
Error from server (BadRequest): container "mongo" in pod "mongo-depl-5fd6b7d4b4-p6jjk" is waiting to start: ContainerCreating

➜  kubectl logs mongo-depl-5fd6b7d4b4-p6jjk
{"t":{"$date":"2021-02-27T05:55:42.939+00:00"},"s":"I",  "c":"CONTROL",  "id":23285,   "ctx":"main","msg":"Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'"}
{"t":{"$date":"2021-02-27T05:55:42.940+00:00"},"s":"W",  "c":"ASIO",     "id":22601,   "ctx":"main","msg":"No TransportLayer configured during NetworkInterface startup"}
{"t":{"$date":"2021-02-27T05:55:42.940+00:00"},"s":"I",  "c":"NETWORK",  "id":4648601, "ctx":"main","msg":"Implicit TCP FastOpen unavailable. If TCP FastOpen is required, set tcpFastOpenServer, tcpFastOpenClient, and tcpFastOpenQueueSize."}
{"t":{"$date":"2021-02-27T05:55:42.941+00:00"},"s":"I",  "c":"STORAGE",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":1,"port":27017,"dbPath":"/data/db","architecture":"64-bit","host":"mongo-depl-5fd6b7d4b4-p6jjk"}}
{"t":{"$date":"2021-02-27T05:55:42.941+00:00"},"s":"I",  "c":"CONTROL",  "id":23403,   "ctx":"initandlisten","msg":"Build Info","attr":{"buildInfo":{"version":"4.4.4","gitVersion":"8db30a63db1a9d84bdcad0c83369623f708e0397","openSSLVersion":"OpenSSL 1.1.1  11 Sep 2018","modules":[],"allocator":"tcmalloc","environment":{"distmod":"ubuntu1804","distarch":"x86_64","target_arch":"x86_64"}}}}
{"t":{"$date":"2021-02-27T05:55:42.941+00:00"},"s":"I",  "c":"CONTROL",  "id":51765,   "ctx":"initandlisten","msg":"Operating System","attr":{"os":{"name":"Ubuntu","version":"18.04"}}}
{"t":{"$date":"2021-02-27T05:55:42.941+00:00"},"s":"I",  "c":"CONTROL",  "id":21951,   "ctx":"initandlisten","msg":"Options set by command line","attr":{"options":{"net":{"bindIp":"*"}}}}
{"t":{"$date":"2021-02-27T05:55:42.941+00:00"},"s":"I",  "c":"STORAGE",  "id":22297,   "ctx":"initandlisten","msg":"Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http:...

```

# getting to pod container shell

```

➜  kubectl exec -it mongo-depl-5fd6b7d4b4-p6jjk -- /bin/bash
root@mongo-depl-5fd6b7d4b4-p6jjk:/# ls
bin  boot  data  dev  docker-entrypoint-initdb.d  etc  home  js-yaml.js  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@mongo-depl-5fd6b7d4b4-p6jjk:/#

```

# deleting deployments

```

➜  kubectl get deployment
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
mongo-depl   1/1     1            1           7m30s
nginx-depl   1/1     1            1           22m
~
➜  kubectl delete deployment mongo-depl
deployment.apps "mongo-depl" deleted
~
➜  kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-depl-7fc44fc5d4-ngbz5   1/1     Running   0          11m
~
➜  kubectl get replicasets
NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-5c8bf76b5b   0         0         0       22m
nginx-depl-7fc44fc5d4   1         1         1       11m
~
➜  kubectl delete deployment nginx-depl
deployment.apps "nginx-depl" deleted
~
➜  kubectl get pods
NAME                          READY   STATUS        RESTARTS   AGE
nginx-depl-7fc44fc5d4-ngbz5   0/1     Terminating   0          11m
~
➜  kubectl get replicasets
No resources found in default namespace.
~
➜  kubectl get pods
No resources found in default namespace.
~
➜  kubectl get deploy
No resources found in default namespace.
~

