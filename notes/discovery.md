# Service Discovery

* the internal DNS service in charge of service discovery and registration can be inspected with the following commands:
```
$ kubectl get svc -n kube-system -l k8s-app=kube-dns
$ kubectl get deploy -n kube-system -l k8s-app=kube-dns
$ kubectl get pods -n kube-system -l k8s-app=kube-dns
```

* the available endpoints objects can be queried:
```
$ kubectl get endpoint ent
```

* we use the following file as example:
````
# cat dns-namespaces.yml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: v1
kind: Namespace
metadata:
  name: prod
---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: enterprise
  namespace: dev
  labels:
    app: enterprise
spec:
  selector:
    matchLabels:
      app: enterprise
  replicas: 2
  template:
    metadata:
      labels:
        app: enterprise
    spec:
      terminationGracePeriodSeconds: 1
      containers:
      - image: nigelpoulton/k8sbook:text-dev
        name: enterprise-ctr
        ports:
        - containerPort: 8080
---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: enterprise
  namespace: prod
  labels:
    app: enterprise
spec:
  selector:
    matchLabels:
      app: enterprise
  replicas: 2
  template:
    metadata:
      labels:
        app: enterprise
    spec:
      terminationGracePeriodSeconds: 1
      containers:
      - image: nigelpoulton/k8sbook:text-prod
        name: enterprise-ctr
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: manchester
  namespace: dev 
  labels:
    event: manchester
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080 
  selector:
    event: manchester
---
apiVersion: v1
kind: Service
metadata:
  name: ent
  namespace: dev
spec:
  ports:
  - port: 8080
  selector:
    app: enterprise
---
apiVersion: v1
kind: Service
metadata:
  name: ent
  namespace: prod
spec:
  ports:
  - port: 8080
  selector:
    app: enterprise
---
apiVersion: v1
kind: Pod
metadata:
  name: jump
  namespace: dev
spec:
  terminationGracePeriodSeconds: 5
  containers:
  - image: ubuntu
    name: jump
    tty: true
    stdin: true
````

* we deploy the configuration:
```
$ kubectl apply -f dns-namespaces.yml
```

* now we can check if it was properly applied, using the '-n' switch to choose between the namespaces:
```
$ kubectl get all -n dev
$ kubectl get all -n prod
```

* we can also connect on the jump container and query the ent service from both namespaces, thanks to kube-dns:
```
$ kubectl exec -it pod/jump -n dev -- bash
root@jump:/# apt update && apt install -y curl
```
* first, we query the ent service of the dev namespace. We only need to mention the service name:
```
root@jump:/# curl ent:8080
```

* first, we query the ent service of the prod namespace. To do so we will need the full FQDN:
```
root@jump:/# curl ent.prod.svc.cluster.local:8080
```
