# CNA --

### training plan 

<img src="plan.png">

### pushing image to docker hub 

```
[ashu@docker-server ashu_apps]$ docker images  |   grep -i ashu
ciscoapps.azurecr.io/commonrepo    ashuappv1.0          e182a92e8d07   19 hours ago   448MB
ashufrontend                       appv1                e182a92e8d07   19 hours ago   448MB
[ashu@docker-server ashu_apps]$ 
[ashu@docker-server ashu_apps]$ 
[ashu@docker-server ashu_apps]$ docker  tag   ashufrontend:appv1  docker.io/dockerashu/ashufrontend:appv1  
[ashu@docker-server ashu_apps]$ 
[ashu@docker-server ashu_apps]$ docker login 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: dockerashu
Password: 
WARNING! Your password will be stored unencrypted in /home/ashu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[ashu@docker-server ashu_apps]$ docker push  docker.io/dockerashu/ashufrontend:appv1
The push refers to repository [docker.io/dockerashu/ashufrontend]
4da3b6f39114: Pushed 
cc1d626a35c4: Pushing [========================>             

[ashu@docker-server ashu_apps]$ docker logout 
Removing login credentials for https://index.docker.io/v1/
```

### k8s cluster setup on prim 

<img src="setup1.png">

## common steps in ALL the VMs

```
[root@ip-172-31-28-99 ~]# 
[root@ip-172-31-28-99 ~]# hostnamectl set-hostname   control-plane 
[root@ip-172-31-28-99 ~]# logout
[ec2-user@ip-172-31-28-99 ~]$ sudo -i
[root@control-plane ~]# hostname
control-plane
[root@control-plane ~]# 


=======>>ENABLE iptables modules 

[root@control-plane ~]# modprobe br_netfilter 
[root@control-plane ~]# echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
[root@control-plane ~]# 

==========> Install any CRE 

root@control-plane ~]# yum install docker  -y  
Failed to set locale, defaulting to C
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
amzn2-core                                                                                                  | 3.7 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package docker.x86_64 0:20.10.13-2.amzn2 will be installed
--> Processing Dependency: runc >= 1.0.0 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: libcgroup >= 0.40.rc1-5.15 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: containerd >= 1.3.2 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: pigz for package: docker-20.10.13-2.amzn2.x86_64
--> Running transaction check

======<<>> If you are using k8s 1.20 + version then only 

[root@control-plane ~]# cat  <<X  >/etc/docker/daemon.json
> {
>   "exec-opts": ["native.cgroupdriver=systemd"]
> }
> 
> X


[root@control-plane ~]# 
[root@control-plane ~]# systemctl daemon-reload 
[root@control-plane ~]# systemctl enable --now docker  # in any version you have to perform 
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@control-plane ~]# 


============> Installing k8s installer tool called Kubeadm -- >?

cat  <<EOF  >/etc/yum.repos.d/kube.repo
> [kube]
> baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
> gpgcheck=0
> EOF


==
yum  install  kubeadm -y  ; systemctl enable --now  kubelet

```

## ONly we need to perform on the system we want to configure as COntrol-plane 


```
root@control-plane ~]# kubeadm  init --pod-network-cidr=192.168.0.0/16  --apiserver-advertise-address=0.0.0.0   --apiserver-cert-extra-sans=3.12.189.181  
[init] Using Kubernetes version: v1.24.3
[preflight] Running pre-flight checks
	[WARNING FileExisting-tc]: tc not found in system path
	[WARNING Hostname]: hostname "control-plane" could not be reached
	[WARNING Hostname]: hostname "control-plane": lookup control-plane on 172.31.0.2:53: no such host
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key

```

### 

```
[root@control-plane ~]# mkdir -p $HOME/.kube
[root@control-plane ~]# cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```

### ONly ON worker Nodes 

```
kubeadm join 172.31.28.99:6443 --token v16x783hv9  --discovery-token-ca-cert-hash sha512:e629457bd08fe9646e
```
