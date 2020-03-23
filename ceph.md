 
### 1.deb

```[ceph_deploy][ERROR ] RuntimeError: Failed to execute command: env DEBIAN_FRONTEND=noninteractive DEBIAN_PRIORITY=critical apt-get --assume-yes -q update```

sudo vim /etc/apt/sources.list.d/ceph.list

https://download.ceph.com/debian-{ceph-stable-release}

to

https://download.ceph.com/debian-luminous

### 2.hostname

connecting to host: ddst-PowerEdge-R8204 resulted in errors: HostNotFound 

vim /etc/hosts

### 3.ssh 警告

ssh name 时有警告
Warning: the ECDSA host key for '[hostname]:port' differs from the key for the IP address '[ipaddress]:port'
Offending key for IP in /home/dsl/.ssh/known_hosts:5
Matching host key in /home/dsl/.ssh/known_hosts:147

解决：ssh-keygen -R （有警告的ip）


[ddst-PowerEdge-R8205][INFO  ] Running command: sudo ceph --version
[ddst-PowerEdge-R8205][DEBUG ] ceph version 12.2.12 (1436006594665279fe734b4c15d7e08c13ebd777) luminous (stable)

### 4.osd active

chown ceph:ceph /var/local/osd0

[global]
fsid = 93dfcccd-729a-423f-80cf-02f57befe495
mon_initial_members = ddst-PowerEdge-R8203, ddst-PowerEdge-R8204, ddst-PowerEdge-R8205
mon_host = 192.168.99.13,192.168.99.14,192.168.99.15
auth_cluster_required = cephx
auth_service_required = cephx
auth_client_required = cephx

### 5. ceph -s 后密钥输出

2020-03-23 22:54:32.327545 7f0a0b394700 -1 auth: unable to find a keyring on /etc/ceph/ceph.client.admin.keyring,/etc/ceph/ceph.keyring,/etc/ceph/keyring,/etc/ceph/keyring.bin,: (2) No such file or directory
2020-03-23 22:54:32.327575 7f0a0b394700 -1 monclient: ERROR: missing keyring, cannot use cephx for authentication
2020-03-23 22:54:32.327579 7f0a0b394700  0 librados: client.admin initialization error (2) No such file or directory
[errno 2] error connecting to the cluster

解决

cp 目录keyring 到 /etc/ceph/

sudo chmod +r /etc/ceph/ceph.client.admin.keyring

### 6.
检查集群健康状态

[root@ceph-3 ceph-ceph-3]# ceph -s
  cluster:
    id:     ea64cc3d-7b7a-4676-b993-df5d71fd7f77
    health: HEALTH_WARN
            no active mgr
 
  services:
    mon: 3 daemons, quorum ceph-1,ceph-2,ceph-3
    mgr: no daemons active
    osd: 3 osds: 3 up, 3 in
 
  data:
    pools:   0 pools, 0 pgs
    objects: 0 objects, 0 bytes
    usage:   3164 MB used, 296 GB / 299 GB avail
    pgs:    

 可以看到health为警告状态，提示我们mgr没有active

为mgr创建用户

 ceph  auth get-or-create mgr.ceph-1 mon 'allow profile mgr' osd 'allow *' mds 'allow *'

查看是否创建mgr用户成功

ceoh auth list

mgr.ceph-1
 key: AQB//PZZbTAzJRAAEakxeHeehHHwbo/AiWTQFg==
 caps: [mds] allow *
 caps: [mon] allow profile mgr
 caps: [osd] allow *

创建mgr的秘钥目录，看启动服务看log时说需要这个文件夹，那么我创建这个文件夹，这里我提前创建，将秘钥导入到里边

mkdir -p  /var/lib/ceph/mgr/ceph-ceph-1/

导入秘钥：

ceph auth get-or-create mgr.ceph-1 -o /var/lib/ceph/mgr/ceph-ceph-1/keyring

也可以把这两步合成一步：

ceph  auth get-or-create mgr.ceph-1 mon 'allow profile mgr' osd 'allow *' mds 'allow *'  -o /var/lib/ceph/mgr/ceph-ceph-1/keyring

授权：

chown -R ceph:ceph /var/lib/ceph/mgr/ceph-ceph-1/*

启动mgr服务：

systemctl restart ceph-mgr@ceph-1


 1926  sudo iptables -A INPUT -i ib0 -p tcp -s 10.0.0.1/24 --dport 6789 -j ACCEPT
 1927  sudo iptables -A INPUT -i ib0 -m multiport -p tcp -s 10.0.0.1/24 --dports 6800:7300 -j ACCEPT
 1941  vim ceph.conf 

 2001  ceph-deploy purge ddst-PowerEdge-R8203 ddst-PowerEdge-R8203 ddst-PowerEdge-R8204 ddst-PowerEdge-R8205

 2005  ceph-deploy new ddst-PowerEdge-R8203 ddst-PowerEdge-R8204 ddst-PowerEdge-R8205
 2008  ceph-deploy install ddst-PowerEdge-R8203 ddst-PowerEdge-R8204 ddst-PowerEdge-R8205 ddst-PowerEdge-R8203 ddst-PowerEdge-R8204 ddst-PowerEdge-R8205
 2009  ceph-deploy mon create-initial

 2021  sudo mkdir /var/local/osd0
 2022  ceph-deploy osd prepare ddst-PowerEdge-R8203:/var/local/osd0 ddst-PowerEdge-R8204:/var/local/osd1 ddst-PowerEdge-R8205:/var/local/osd2
 2024  ceph-deploy osd prepare ddst-PowerEdge-R8203:/var/local/osd0 ddst-PowerEdge-R8204:/var/local/osd1 ddst-PowerEdge-R8205:/var/local/osd2
 2025  ceph-deploy osd activate ddst-PowerEdge-R8203:/var/local/osd0 ddst-PowerEdge-R8204:/var/local/osd1 ddst-PowerEdge-R8205:/var/local/osd2
 2026  sudo chown ceph:ceph /var/local/osd0
 2027  ceph-deploy osd activate ddst-PowerEdge-R8203:/var/local/osd0 ddst-PowerEdge-R8204:/var/local/osd1 ddst-PowerEdge-R8205:/var/local/osd2
 2031  ceph-deploy admin ddst-PowerEdge-R8203 ddst-PowerEdge-R8204 ddst-PowerEdge-R8205 
 2035  sudo chmod +r /etc/ceph/ceph.client.admin.keyring
 2042  ceph-deploy admin ddst-PowerEdge-R8203 ddst-PowerEdge-R8203
 2043  ceph -s
 2044  sudo chmod +r /etc/ceph/ceph.client.admin.keyring
 2050  ceph health detail
 2052  ceph health detail
 2053  ceph-deploy osd activate ddst-PowerEdge-R8203:/var/local/osd0 ddst-PowerEdge-R8204:/var/local/osd1 ddst-PowerEdge-R8205:/var/local/osd2
 2057  ceph-deploy --overwrite-conf admin ddst-PowerEdge-R8203 ddst-PowerEdge-R8204 ddst-PowerEdge-R8205 ddst-PowerEdge-R8203
 2059  ceph-deploy mgr create ddst-PowerEdge-R8203 ddst-PowerEdge-R8204 ddst-PowerEdge-R8205
 2061  find / -name ceph.client.admin.keyring
 2062  sudo find / -name ceph.client.admin.keyring
 2063  cd /etc/ceph/
 2064  ls
 2065  vim ceph.client.admin.keyring 
 2066  dpkg-divert --local --rename --add /sbin/initctl
 2067  sudo dpkg-divert --local --rename --add /sbin/initctl
 2068  sudo ln -snf /bin/true /sbin/initctl
 2069  sudo rm -f /etc/apt/sources.list.d/ceph.list
 2070  modprobe ceph
 2071  sudo modprobe ceph
 2072  ceph-deploy admin ddst-PowerEdge-R8203
 2073  cd ~/cluster/
 2074  ls
 2075  ceph-deploy admin ddst-PowerEdge-R8203
 2076  ceph osd tree
 2077  chown -R ceph:ceph /var/lib/ceph/
 2078  sudo chown -R ceph:ceph /var/lib/ceph/
 2079  sudo chown -R ceph:ceph ~/ceph/
 2080  sudo chown -R ceph:ceph /var/local/osd0/
 2081  ceph-deploy osd activate ddst-PowerEdge-R8203:/var/local/osd0
 2082  ceph-deploy osd activate ddst-PowerEdge-R8204:/var/local/osd1
 2083  ceph-deploy osd activate ddst-PowerEdge-R8205:/var/local/osd2
 2084  ceph-deploy admin ddst-PowerEdge-R8203

 2089  ceph-deploy admin ddst-PowerEdge-R8203 ddst-PowerEdge-R8203 ddst-PowerEdge-R8204 ddst-PowerEdge-R8205
 2090  ceph health
 2091  sudo chmod +r /etc/ceph/ceph.client.admin.keyring
 2092  cd /etc/ceph/
 2093  ls
 2094  ceph health
 2095  ceph -s
 2096  ceph osd tree
 2097  ceph auth get-key client.admin | base64



https://blog.csdn.net/don_chiang709/article/details/91511828#1.%20%5Bceph_deploy%5D%5BERROR%20%5D%20RuntimeError%3A%20Failed%20to%20execute%20command%3A%20env%20DEBIAN_FRONTEND%3Dnoninteractive%20DEBIAN_PRIORITY%3Dcritical%20apt-get%20--assume-yes%20-q%20update
