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



