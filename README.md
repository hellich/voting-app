# voting-app using PODs

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

# voting-app using deployments

```
helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f worker-app-deploy.yaml
deployment.apps/worker-app-deploy created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f result-app-deploy.yaml
deployment.apps/result-app-deploy created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl create -f result-app-service.yaml
service/result-service created

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods,svc
NAME                                     READY   STATUS              RESTARTS   AGE
pod/postgres-deploy-6cb68bd648-d8jkx     1/1     Running             0          72s
pod/redis-deploy-598c448457-hmksr        1/1     Running             0          83s
pod/result-app-deploy-6f8485755-cvxmh    0/1     ContainerCreating   0          6s
pod/voting-app-deploy-5cb7bc8558-g2792   1/1     Running             0          113s
pod/worker-app-deploy-6dbf88866b-bwfn2   0/1     ContainerCreating   0          13s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/db               ClusterIP   10.96.57.231     <none>        5432/TCP       68s
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        60m
service/redis            ClusterIP   10.105.154.241   <none>        6379/TCP       80s
service/result-service   NodePort    10.99.56.179     <none>        80:30005/TCP   2s
service/voting-service   NodePort    10.104.114.242   <none>        80:30004/TCP   109s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get deployments,svc
NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/postgres-deploy     1/1     1            1           81s
deployment.apps/redis-deploy        1/1     1            1           92s
deployment.apps/result-app-deploy   0/1     1            0           15s
deployment.apps/voting-app-deploy   1/1     1            1           2m2s
deployment.apps/worker-app-deploy   0/1     1            0           22s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/db               ClusterIP   10.96.57.231     <none>        5432/TCP       77s
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        60m
service/redis            ClusterIP   10.105.154.241   <none>        6379/TCP       89s
service/result-service   NodePort    10.99.56.179     <none>        80:30005/TCP   11s
service/voting-service   NodePort    10.104.114.242   <none>        80:30004/TCP   118s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get deployments,svc
NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/postgres-deploy     1/1     1            1           83s
deployment.apps/redis-deploy        1/1     1            1           94s
deployment.apps/result-app-deploy   0/1     1            0           17s
deployment.apps/voting-app-deploy   1/1     1            1           2m4s
deployment.apps/worker-app-deploy   0/1     1            0           24s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/db               ClusterIP   10.96.57.231     <none>        5432/TCP       79s
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        60m
service/redis            ClusterIP   10.105.154.241   <none>        6379/TCP       91s
service/result-service   NodePort    10.99.56.179     <none>        80:30005/TCP   13s
service/voting-service   NodePort    10.104.114.242   <none>        80:30004/TCP   2m

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get deployments,svc
NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/postgres-deploy     1/1     1            1           86s
deployment.apps/redis-deploy        1/1     1            1           97s
deployment.apps/result-app-deploy   0/1     1            0           20s
deployment.apps/voting-app-deploy   1/1     1            1           2m7s
deployment.apps/worker-app-deploy   0/1     1            0           27s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/db               ClusterIP   10.96.57.231     <none>        5432/TCP       82s
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        60m
service/redis            ClusterIP   10.105.154.241   <none>        6379/TCP       94s
service/result-service   NodePort    10.99.56.179     <none>        80:30005/TCP   16s
service/voting-service   NodePort    10.104.114.242   <none>        80:30004/TCP   2m3s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get deployments,svc
NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/postgres-deploy     1/1     1            1           87s
deployment.apps/redis-deploy        1/1     1            1           98s
deployment.apps/result-app-deploy   0/1     1            0           21s
deployment.apps/voting-app-deploy   1/1     1            1           2m8s
deployment.apps/worker-app-deploy   0/1     1            0           28s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/db               ClusterIP   10.96.57.231     <none>        5432/TCP       83s
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        60m
service/redis            ClusterIP   10.105.154.241   <none>        6379/TCP       95s
service/result-service   NodePort    10.99.56.179     <none>        80:30005/TCP   17s
service/voting-service   NodePort    10.104.114.242   <none>        80:30004/TCP   2m4s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get deployments,svc
NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/postgres-deploy     1/1     1            1           88s
deployment.apps/redis-deploy        1/1     1            1           99s
deployment.apps/result-app-deploy   0/1     1            0           22s
deployment.apps/voting-app-deploy   1/1     1            1           2m9s
deployment.apps/worker-app-deploy   0/1     1            0           29s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/db               ClusterIP   10.96.57.231     <none>        5432/TCP       84s
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        60m
service/redis            ClusterIP   10.105.154.241   <none>        6379/TCP       96s
service/result-service   NodePort    10.99.56.179     <none>        80:30005/TCP   18s
service/voting-service   NodePort    10.104.114.242   <none>        80:30004/TCP   2m5s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods
NAME                                 READY   STATUS              RESTARTS   AGE
postgres-deploy-6cb68bd648-d8jkx     1/1     Running             0          95s
redis-deploy-598c448457-hmksr        1/1     Running             0          106s
result-app-deploy-6f8485755-cvxmh    0/1     ContainerCreating   0          29s
voting-app-deploy-5cb7bc8558-g2792   1/1     Running             0          2m16s
worker-app-deploy-6dbf88866b-bwfn2   0/1     ContainerCreating   0          36s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods
NAME                                 READY   STATUS              RESTARTS   AGE
postgres-deploy-6cb68bd648-d8jkx     1/1     Running             0          98s
redis-deploy-598c448457-hmksr        1/1     Running             0          109s
result-app-deploy-6f8485755-cvxmh    0/1     ContainerCreating   0          32s
voting-app-deploy-5cb7bc8558-g2792   1/1     Running             0          2m19s
worker-app-deploy-6dbf88866b-bwfn2   0/1     ContainerCreating   0          39s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods
NAME                                 READY   STATUS              RESTARTS   AGE
postgres-deploy-6cb68bd648-d8jkx     1/1     Running             0          99s
redis-deploy-598c448457-hmksr        1/1     Running             0          110s
result-app-deploy-6f8485755-cvxmh    0/1     ContainerCreating   0          33s
voting-app-deploy-5cb7bc8558-g2792   1/1     Running             0          2m20s
worker-app-deploy-6dbf88866b-bwfn2   0/1     ContainerCreating   0          40s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods
NAME                                 READY   STATUS              RESTARTS   AGE
postgres-deploy-6cb68bd648-d8jkx     1/1     Running             0          102s
redis-deploy-598c448457-hmksr        1/1     Running             0          113s
result-app-deploy-6f8485755-cvxmh    0/1     ContainerCreating   0          36s
voting-app-deploy-5cb7bc8558-g2792   1/1     Running             0          2m23s
worker-app-deploy-6dbf88866b-bwfn2   0/1     ContainerCreating   0          43s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
postgres-deploy-6cb68bd648-d8jkx     1/1     Running   0          5m26s
redis-deploy-598c448457-hmksr        1/1     Running   0          5m37s
result-app-deploy-6f8485755-cvxmh    1/1     Running   0          4m20s
voting-app-deploy-5cb7bc8558-g2792   1/1     Running   0          6m7s
worker-app-deploy-6dbf88866b-bwfn2   1/1     Running   0          4m27s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ minikube service voting-service --url
http://192.168.59.104:30004

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ minikube service result-service --url
http://192.168.59.104:30005

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl scale deployment voting-app-deploy --replicas=3
deployment.apps/voting-app-deploy scaled

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
postgres-deploy-6cb68bd648-d8jkx     1/1     Running   0          7m6s
redis-deploy-598c448457-hmksr        1/1     Running   0          7m17s
result-app-deploy-6f8485755-cvxmh    1/1     Running   0          6m
voting-app-deploy-5cb7bc8558-98bn9   1/1     Running   0          4s
voting-app-deploy-5cb7bc8558-g2792   1/1     Running   0          7m47s
voting-app-deploy-5cb7bc8558-v6jtt   1/1     Running   0          4s
worker-app-deploy-6dbf88866b-bwfn2   1/1     Running   0          6m7s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
$ kubectl get deployments
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
postgres-deploy     1/1     1            1           7m14s
redis-deploy        1/1     1            1           7m25s
result-app-deploy   1/1     1            1           6m8s
voting-app-deploy   3/3     3            3           7m55s
worker-app-deploy   1/1     1            1           6m15s

helmi@DESKTOP-HDTIT2C MINGW64 /d/code/voting-app (main)
```