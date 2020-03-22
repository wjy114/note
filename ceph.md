 
### 1.deb

```[ceph_deploy][ERROR ] RuntimeError: Failed to execute command: env DEBIAN_FRONTEND=noninteractive DEBIAN_PRIORITY=critical apt-get --assume-yes -q update```

sudo vim /etc/apt/sources.list.d/ceph.list

https://download.ceph.com/debian-{ceph-stable-release}

to

https://download.ceph.com/debian-luminous

### 2.hostname

connecting to host: ddst-PowerEdge-R8204 resulted in errors: HostNotFound 

vim /etc/hosts

### 3. ssh 警告

ssh name 时有警告
Warning: the ECDSA host key for '[hostname]:port' differs from the key for the IP address '[ipaddress]:port'
Offending key for IP in /home/dsl/.ssh/known_hosts:5
Matching host key in /home/dsl/.ssh/known_hosts:147

解决：ssh-keygen -R （有警告的ip）


[ddst-PowerEdge-R8205][INFO  ] Running command: sudo ceph --version
[ddst-PowerEdge-R8205][DEBUG ] ceph version 12.2.12 (1436006594665279fe734b4c15d7e08c13ebd777) luminous (stable)


https://blog.csdn.net/don_chiang709/article/details/91511828#1.%20%5Bceph_deploy%5D%5BERROR%20%5D%20RuntimeError%3A%20Failed%20to%20execute%20command%3A%20env%20DEBIAN_FRONTEND%3Dnoninteractive%20DEBIAN_PRIORITY%3Dcritical%20apt-get%20--assume-yes%20-q%20update
