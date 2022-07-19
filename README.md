# CNA --

### training plan 

<img src="plan.png">

### monolith break 

<img src="break_mono.png">

### problems with vm 

<img src="probvm2.png">

### container with app 

<img src="cont1.png">

### Installing docker on LInux vM 

```
[root@docker-server ~]# yum install  docker  -y 
Failed to set locale, defaulting to C
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
Resolving Dependencies
--> Running transaction check
---> Package docker.x86_64 0:20.10.13-2.amzn2 will be installed
--> Processing Dependency: runc >= 1.0.0 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: l
```

### creating users for docker access 

```
 36  for  i in  ashu arun eric inder ishrak john lucy naveen oliver rahul siva ujjwal ; do useradd $i; echo "Cisco@1234#"  |  passwd $i --stdin ; usermod -aG docker  $i; done 
   37  systemctl enable --now docker 
   

```

### lets connect to docker engine 

```
[ashu@docker-server ~]$ docker  version 
Client:
 Version:           20.10.13
 API version:       1.41
 Go version:        go1.16.15
 Git commit:        a224086
 Built:             Thu Mar 31 19:20:32 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.13
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.15
  Git commit:       906f57f
  Built:            Thu Mar 31 19:21:13 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.13
  GitCommit:        9cc61520f4cd876b86e77edfeb88fbcd536d1f9d
 runc:
  Version:          1.0.3
  GitCommit:        f46b6ba2c9314cfc8ca
```
### COntainer based application running 

<img src="apprun.png">


