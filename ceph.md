 
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
