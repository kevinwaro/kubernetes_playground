# Services

* we take as example the file svc.yml.
```
# cat svc.yml
apiVersion: v1
kind: Service
metadata:
  name: hello-svc
  labels:
    app: hello-world
spec:
  type: NodePort
  ports:
  - port: 8080
    nodePort: 30001
    protocol: TCP
  selector:
    app: hello-world
```

* we deploy the file:
```
$ kubectl apply -f svc.yml
```

* Now that the service is deployed, the app is available:
 * from inside the cluster, at http://10.106.108.73:8080 (from master)
 * from outside the cluster at http://172.16.1.12:30001 (from anywhere else)
