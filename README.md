# CNA --

### training plan 

<img src="plan.png">

### cleaning namespace

```
[ashu@docker-server ~]$ kubectl delete all --all
pod "ashufrontend-66ccfb99c-25g2r" deleted
pod "ashufrontend-66ccfb99c-9pfkl" deleted
pod "ashufrontend-66ccfb99c-jr4sk" deleted
pod "mydep1-5994b4566d-vxrqq" deleted
pod "net-test" deleted
service "ashufrontend-lb" deleted
deployment.apps "ashufrontend" deleted
deployment.apps "mydep1" deleted
[ashu@docker-server ~]$ 
[ashu@docker-server ~]$ kubectl  get  deploy 
No resources found in ashu-apps namespace.
[ashu@docker-server ~]$ kubectl  get po
No resources found in ashu-apps namespace.
[ashu@docker-server ~]$ kubectl  get svc
No resources found in ashu-apps namespace.
[ashu@docker-server ~]$ 
```

### lets Design  app and deploy in k8s

<img src="app_design.png">

### Your storage will be planning remote storage you 

### demo NFS 

```

```

## webapp Deploy of -- Db 

### creating Deployment 

```
kubectl   create  deployment  ashudb  --image=mysql:5.6  --port 3306 --dry-run=client  -o yaml >db_deploy.yaml
```

### COnfiguration Details 

```
create  configmap  db-details  --from-literal MYSQL_USER="admin" --from-literal MYSQL_DATABASE="webapp"  --dry-run=client -o yaml  >db_configmap.yaml
```

### storing db credentials 

```
kubectl  create  secret generic  db-cred  --from-literal  sqlpass="Cisco@123#" --from-literal rootpass="Cisco@123#" --dry-run=client -o yaml  >db_secret.yaml
```

### Deployment file final 

```
[ashu@docker-server myapp]$ cat  db_deploy.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashudb
  name: ashudb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashudb
  strategy: {}
  template: # template section 
    metadata:
      creationTimestamp: null
      labels:
        app: ashudb
    spec:
      volumes: # to create volume from some source 
      - name: ashudb-vol
        hostPath:
         path: /db/ashu/ 
         type: DirectoryOrCreate
      containers:
      - image: mysql:5.6
        name: mysql
        ports:
        - containerPort: 3306
        volumeMounts: # to attach volume to the container 
        - name: ashudb-vol
          mountPath: /var/lib/mysql/ # defautl location where you store db 
        envFrom: # call from config details 
        - configMapRef:
            name: db-details 
        env: # call/ change env variable 
        - name: MYSQL_PASSWORD
          valueFrom: # reading value from some reference 
            secretKeyRef: # secret is the reference 
              name: db-cred 
              key: sqlpass
        - name: MYSQL_ROOT_PASSWORD
          valueFrom: # reading value from some reference 
            secretKeyRef: # secret is the reference 
              name: db-cred 
              key: rootpass
        resources: {}
status: {}

```

### final deploy of db section 

```
[ashu@docker-server myapp]$ kubectl replace -f . --force 
configmap "db-details" deleted
deployment.apps "ashudb" deleted
secret "db-cred" deleted
configmap/db-details replaced
deployment.apps/ashudb replaced
secret/db-cred replaced
[ashu@docker-server myapp]$ kubectl  get  deploy 
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
ashudb   1/1     1            1           9s
[ashu@docker-server myapp]$ kubectl  get  cm
NAME               DATA   AGE
db-details         2      14s
kube-root-ca.crt   1      20h
[ashu@docker-server myapp]$ kubectl  get secret
NAME      TYPE     DATA   AGE
db-cred   Opaque   2      17s

```

### creating Internal LB for Db 

```
[ashu@docker-server ~]$ kubectl  get  deploy 
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
ashudb   1/1     1            1           38m
[ashu@docker-server ~]$ kubectl expose deployment ashudb  --type ClusterIP --port 3306  --name ashudb-lb --dry-run=client -o yaml   >dbsvc.yaml

[ashu@docker-server myapp]$ kubectl apply -f  dbsvc.yaml 
service/ashudb-lb created
[ashu@docker-server myapp]$ kubectl  get  svc
NAME        TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
ashudb-lb   ClusterIP   10.99.132.76   <none>        3306/TCP   3s
[ashu@docker-server myapp]$ 


```

## webapp Deployment steps 

### deloyment file 

```
kubectl create  deployment  frontend  --image=wordpress:4.8-apache --port 80 --dry-run=client -o yaml    >web_deploy.yaml 
```

### db connect details 

```
 kubectl  create configmap  web-db-conn --from-literal  WORDPRESS_DB_USER="admin" --from-literal  WORDPRESS_DB_NAME="webapp" --from-literal WORDPRESS_DB_HOST="ashudb-lb"   --dry-run=client -o yaml >web_config.yaml
```
