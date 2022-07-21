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
create  configmap  db_details  --from-literal MYSQL_USER="admin" --from-literal MYSQL_DATABASE="webapp"  --dry-run=client -o yaml  >db_configmap.yaml
```

### storing db credentials 

```
kubectl  create  secret generic  db_cred  --from-literal  sqlpass="Cisco@123#"  --dry-run=client -o yaml   >db_secret.yaml
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
      volumes: 
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
            name: db_details 
        env: # call/ change env variable 
        - name: MYSQL_PASSWORD
          valueFrom: # reading value from some reference 
            secretKeyRef: # secret is the reference 
              name: db_cred 
              key: sqlpass
        resources: {}
status: {}

```

### final deploy of db section 

```
[ashu@docker-server myapp]$ kubectl  apply   -f   . 
configmap/db-details created
deployment.apps/ashudb created
secret/db-cred created
[ashu@docker-server myapp]$ kubectl  get  configmap 
NAME               DATA   AGE
db-details         2      10s
kube-root-ca.crt   1      20h
[ashu@docker-server myapp]$ kubectl  get  cm
NAME               DATA   AGE
db-details         2      14s
kube-root-ca.crt   1      20h
[ashu@docker-server myapp]$ kubectl  get  secret
NAME      TYPE     DATA   AGE
db-cred   Opaque   1      21s
[ashu@docker-server myapp]$ kubectl  get deploy 
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
ashudb   0/1     1            0           28s
[ashu@docker-server myapp]$ kubectl  get  po
NAME                      READY   STATUS              RESTARTS   AGE
ashudb-5f8fc8c47f-wqwp6   0/1     ContainerCreating   0          36s
[ashu@docker-server myapp]$ 



```

