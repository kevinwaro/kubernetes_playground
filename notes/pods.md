# Pods

* let's take as example the file pod.yml:
```
# $ cat pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod
  labels:
    zone: prod
    version: v1
spec:
  containers:
  - name: hello-ctr
    image: nigelpoulton/k8sbook:latest
    ports:
    - containerPort: 8080
```

* create the pod:
```
$ kubectl apply -f pod.yml
```

* display cluster info:
```
$ kubectl get all
k$ ubectl get nodes
```

* display infos about the pod:
```
$ kubectl get pods hello-pod -o yaml
$ kubectl describe pods hello-pod 
```

* run a command from inside the pod:
```
$ kubectl exec hello-pod -- ps aux
$ kubectl exec -it hello-pod -- sh
$ kubectl exec -it hello-pod --container hello-ctr -- sh
```

* display logs of the pod:
```
$ kubectl logs hello-pod
$ kubectl logs hello-pod -f
$ kubectl logs hello-pod --container hello-ctr
```

* destroy the pod:
```
$ kubectl delete -f pod.yml
```
