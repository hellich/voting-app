# voting-app

```
#create voting pod
$ kubectl create -f voting-app-pod.yaml

#create voting service
$ kubectl create -f voting-app-service.yaml

```

Then

```
helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods
NAME             READY   STATUS              RESTARTS   AGE
voting-app-pod   0/1     ContainerCreating   0          12s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods,svc
NAME                 READY   STATUS    RESTARTS   AGE
pod/voting-app-pod   1/1     Running   0          22s

NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes       ClusterIP   10.96.0.1       <none>        443/TCP        31m
service/voting-service   NodePort    10.109.167.63   <none>        80:30004/TCP   19s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ minikube service voting-service --url
http://192.168.59.103:30004

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f redis-pod.yaml
pod/redis-pod created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f redis-service.yaml
service/redis created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods,svc
NAME                 READY   STATUS    RESTARTS   AGE
pod/redis-pod        1/1     Running   0          15s
pod/voting-app-pod   1/1     Running   0          33m

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        63m
service/redis            ClusterIP   10.103.217.220   <none>        6379/TCP       11s
service/voting-service   NodePort    10.109.167.63    <none>        80:30004/TCP   33m

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f postgres-pod.yaml
pod/postgres-pod created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f postgres-service.yaml
service/db created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f worker-pod.yaml
pod/worker-app-pod created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f result-app-pod.yaml
pod/result-app-pod created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f result-app-service.yaml
service/result-service created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods,svc
NAME                 READY   STATUS    RESTARTS        AGE
pod/postgres-pod     1/1     Running   0               110s
pod/redis-pod        1/1     Running   0               15m
pod/result-app-pod   1/1     Running   1 (2m16s ago)   10m
pod/voting-app-pod   1/1     Running   0               48m
pod/worker-app-pod   1/1     Running   0               98s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/db               ClusterIP   10.110.194.14    <none>        5432/TCP       105s
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        79m
service/redis            ClusterIP   10.103.217.220   <none>        6379/TCP       15m
service/result-service   NodePort    10.109.198.81    <none>        80:30005/TCP   10m
service/voting-service   NodePort    10.109.167.63    <none>        80:30004/TCP   48m

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl logs worker-app-pod
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Waiting for db
Connected to db
Found redis at 10.103.217.220
Connecting to redis

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ minikube service result-service --url
http://192.168.59.103:30005


```