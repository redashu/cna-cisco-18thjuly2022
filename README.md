# CNA --

### training plan 

<img src="plan.png">

### problems in app testing / deployment in past 

<img src="probday1.png">

### wastage of resources due to app libs confict with Host OS 

<img src="appconfday1.png">

### Introduction to Virtulailiztion using Hypervisor 

<img src="vmday1.png">

### type 1 and type 2 Hypervisors 

<img src="day1hy.png">

### to avoid manaual management we can adopt / outsource resources -- managed by others 

<img src="cnaday1.png">

## intro to Virtual / cloud computing services 

### Cloud Deployments Model -- for Providers 

<img src="deployday1.png">
 
### cloud delivery models 

<img src="deployday1.png">

### IAAS -- CNA model 

<img src="day1rg.png">

### launching instance in aws cloud using ec2 service and accessing it via ssh 

```
fire@ashutoshhs-MacBook-Air ~ % ls  -l Downloads/ashuciscokey.pem 
-rw-r--r--@ 1 fire  staff  1704 Jul 18 12:17 Downloads/ashuciscokey.pem
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % chmod 400 Downloads/ashuciscokey.pem   ## Don't required in Windows powershell 
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % ls  -l Downloads/ashuciscokey.pem    
-r--------@ 1 fire  staff  1704 Jul 18 12:17 Downloads/ashuciscokey.pem
fire@ashutoshhs-MacBook-Air ~ % 
fire@ashutoshhs-MacBook-Air ~ % ssh  -i Downloads/ashuciscokey.pem  ec2-user@18.218.165.169 
The authenticity of host '18.218.165.169 (18.218.165.169)' can't be established.
ECDSA key fingerprint is SHA256:bjN3EsKtxZY54ua/HUcVefqcTaJCeX99l6pnPUVZZIs.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '18.218.165.169' (ECDSA) to the list of known hosts.

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
5 package(s) needed for security, out of 14 available
Run "sudo yum update" to apply all updates.
-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
[ec2-user@ip-172-31-32-163 ~]$ 
```






