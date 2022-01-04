# Deployments

* There are 2 ways to declare a NodePort object. The declarative and the imperative way.

* We use as example the following deploy.yml file:
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

* First the imperative way:
```
kubectl expose deployment hello-deploy --name=hello-svc --target-port=8080 --type=NodePort
```

* This will create an NodePort object and expose hello-deploy. However, it is not the recommented way to do it.

* Now the declarative way. Create the following svc.yml file:
```
# cat svc.yml
apiVersion: v1
kind: Service
metadata:
  name: hello-svc
spec:
  type: NodePort
  ports:
  - port: 8080
    nodePort: 30001
    targetPort: 8080
    protocol: TCP
  selector:
    app: hello-world
```

* Then apply the file:
```
kubectl apply -f svc.yml
```

* Now that the file is applied you can inspect it:
```
kubectl get svc hello-svc
kubectl describe svc hello-svco
```

* As every Service has its own Endpoints object, you can inspect it as well:
```
kubectl get ep hello-svc
kubectl decribe ep hello-svc
```
