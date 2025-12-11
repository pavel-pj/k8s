# k8s

#### Config kubectl :
```bash
kubectl config view
```

####  checking status of minikube
```bash
 minikube status
```



2. replicas


#### delete
```bash
kubectl delete replicaset my-replica-name
```
 
#### increase pods in replica
```bash
kubectl scale replicaset my-replicaset --replicas 3
```

#### edit
```bash
kubectl edit replicaset my-replicaset 
```

#### set params to replica
```bash
kubectl set image replicaset my-replicaset nginx=nginx:1.21
```

#### get a parametr as json
```bash
kubectl get po my-replicaset-fe43d -o=jsonpath='{.spec.containers[*].image}{"\n"}'
```