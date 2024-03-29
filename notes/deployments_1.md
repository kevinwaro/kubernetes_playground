# Deployments

* we take as example the file deploy.yml:
```
# $ cat deploy.yml
apiVersion: apps/v1 #Older versions of k8s use apps/v1beta1
kind: Deployment
metadata:
  name: hello-deploy
spec:
  replicas: 10
  selector:
    matchLabels:
      app: hello-world
  minReadySeconds: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-pod
        image: nigelpoulton/k8sbook:latest
        ports:
        - containerPort: 8080
```

* we deploy it:
```
$ kubectl apply -f deploy.yml
```

* there are many ways to inspect deployments:
```
$ kubectl get deploy hello-world
$ kubectl describe deploy hello-deploy
```

* to confirm is replica sets were created:
```
$ kubectl get rs
```
