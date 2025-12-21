# k8s

#### Config kubectl :
```bash
kubectl config view
```

####  checking status of minikube
```bash
 minikube status
```



### 2. replicaSet


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

### 3. Deployment
#### set params to deployment
```bash
kubectl set image deployment my-deployment '*=nginx:1.13'
```


#### 4. Namespaces

#### get system namespaces
```bash
kubectl -n kube-system get pod
```

#### create
```bash
kubectl create ns student
```
#### creating deploy in the new namespace
```bash
kubectl -n student apply -f deployment.yaml 
```
#### get po from namespace 
```bash
kubectl -n student get pod
```



## CONFIGURATIONS 
#### get all env from POD
```bash
kubectl exec -it my-deployment-6f95497496-bztpj  -- env
```

#### create configmap
```bash
kubectl create -f configmap.yaml
```

#### see config 
```bash
kubectl get cm my-configmap-env -o yaml
```
#### or : 
```bash
kubectl describe cm my-configmap-env
```

#### port-forwar
allow to see 80 port in localhost 
```bash
kubectl port-forward my-deployment-6cb87ccf6c-8nc5v 8080:80 &
curl 127.0.0.1:8080
```


## 3.pvc 

#### Создает PersistentVolumeClaim в Minikube

```bash
kubectl apply -f .
```

#### данные будут связаны черех pod, /data ( это видно в deployment volume)
```bash
kubectl get pod
kubectl exec -it <pod-name> -- bash
touch /data/test-file
```

#### файл будет сохранен на физической ноде, то есть в миникубе
#### мы увидим файл который был создан в кластере
```bash
minikube ssh
ls -l /local/pv1
```

#### при удалении pod и пересоздании volume сохраняется
#### файл будет виден и на ноде и в кластере 
```bash
kubectl delete .
kubectl apply -f .
``` 



## SERVICES
#### should be seen probabilities - 0.5000 - if 2 pods only ONE time, if 3 pods  - 0.333 - twice
```bash
minikube ssh
iptables -t nat -S
```
#### shows all pods with IP
```bash
kubectl get pods -o wide
```

### create noPORT service
#### you can connect to application using a port from 30000 - 32767
```bash
kubectl create -f nodeport.yaml
kubectl get svc 
my-service-np   NodePort    10.101.26.244   <none>        80:31596/TCP   8s
curl $(minikube ip):31596
```



Способ 1: Добавить в /etc/hosts (самый простой)
bash

# Узнайте IP Minikube
MINIKUBE_IP=$(minikube ip)
echo "Minikube IP: $MINIKUBE_IP"

# Добавьте запись (требует sudo)
echo "$MINIKUBE_IP myapp.local" | sudo tee -a /etc/hosts

# Проверьте
ping -c 1 myapp.local


