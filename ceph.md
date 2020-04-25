 
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


mkdir ceph-cluster

cd ceph-cluster/

ceph-deploy new 1

osd pool default size = 2

ceph-deploy install 1 1 2 3

ceph-deploy mon create-initial

ssh c2

sudo mkdir /var/local/osd0

sudo chown -R ceph:ceph /var/local/osd0

exit

ssh c3

sudo mkdir /var/local/osd1

sudo chown -R ceph:ceph /var/local/osd1

exit

ceph-deploy osd prepare c2:/var/local/osd0 c3:/var/local/osd1

ssh c2

sudo chown -R ceph:ceph /var/lib/ceph

exit

ssh c3

sudo chown -R ceph:ceph /var/lib/ceph

exit

ceph-deploy osd activate c2:/var/local/osd0 c3:/var/local/osd1

ceph-deploy admin 1 1 2 3

ssh 1 2 3

sudo chmod +r /etc/ceph/ceph.client.admin.keyring

ceph osd tree

ceph  auth get-or-create mgr.ceph-1 mon 'allow profile mgr' osd 'allow *' mds 'allow *'

ceph auth list

mkdir -p /var/lib/ceph/mgr/ceph-ceph-1

sudo mkdir -p /var/lib/ceph/mgr/ceph-ceph-1

sudo ceph auth get-or-create mgr.ceph-1 -o /var/lib/ceph/mgr/ceph-ceph-1/keyring

sudo chown -R ceph:ceph /var/lib/ceph/mgr/

sudo systemctl restart ceph-mgr@ceph-1

ceph -s

生成mgr密钥

ceph auth get-or-create mgr.{hostname} mon 'allow *' osd 'allow *'

2、创建mgr数据目录

mkdir /var/lib/ceph/mgr/ceph-{hostname}/

3、保存密钥文件

ceph auth get mgr.{hostname} -o /var/lib/ceph/mgr/ceph-{hostname}/keyring

4、启动mgr

使用 ceph-mgr -i {hostname} 可以对mgr进行启动，例如： ceph-mgr -i node1 。启动之后通过ceph -s 
可以看到mgr active的信息。


ssh 1 "ceph auth get-key client.admin | base64"

ssh 1 "mkdir -p /var/lib/ceph/mds/ceph-$MDS"

ssh 1"chown -R ceph:ceph /var/lib/ceph"



ssh $MDS ceph osd pool create fs_db_data 128 

ssh $MDS ceph osd pool create fs_db_metadata 64

ceph osd lspools

ceph-deploy --overwrite-conf mds create $MDS

# 挂载需要的验证秘钥

MOUNTKEY=`ssh $MDS "ceph auth get-key client.admin"`

# 节点ip

MONIP=`ssh $MDS cat /etc/ceph/ceph.conf |grep mon_host|cut -d "=" -f2|sed 's?,?:6789,?g'`

# 挂载目录

mkdir /mycephfs

# 开始挂载

mount -t ceph $MONIP:6789:/ /mycephfs -o name=admin,secret=$MOUNTKEY

 
查看挂载情况

root@jqb-node128:~/cephinstall# df -hT

https://blog.csdn.net/don_chiang709/article/details/91511828#1.%20%5Bceph_deploy%5D%5BERROR%20%5D%20RuntimeError%3A%20Failed%20to%20execute%20command%3A%20env%20DEBIAN_FRONTEND%3Dnoninteractive%20DEBIAN_PRIORITY%3Dcritical%20apt-get%20--assume-yes%20-q%20update

git xio

Unable to find the numactl-devel header files
sudo apt-get install libnuma-dev
Unable to find the libaio-devel header fi
sudo apt install libaio-dev

ceph
sudo apt-get install libsnappy1 libsnappy-dev

sudo apt install libleveldb-dev

sudo apt-get install libblkid-dev

sudo apt-get install libudev-dev

sudo apt-get install libexpat-dev

sudo apt install libkeyutils-dev

sudo apt-get install libcrypto++-dev

sudo apt-get install libboost-regex1.62-dev

sudo apt-get install libboost-thread-dev

sudo apt-get install libboost-all-dev

sudo apt-get install xfslibs-dev

sudp apt-get install xfslibs-dev

sudo apt-get install libatomic-ops-dev


CXXFLAGS="-I/opt/accelio/include" CFLAGS="-I/opt/accelio/include" LDFLAGS="-L/opt/accelio/lib" ./configure --prefix=/opt/ceph --enable-xio  --without-tcmalloc 

sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev
sudo apt-get install libssl-dev -y
sudo apt-get install libevent-dev -y
sudo apt-get install libtool
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install libdb4.8-dev libdb4.8++-dev
sudo make -j64
sudo apt-get install -y python-setuptools

yum install libunwind-devel

wget https://github.com/gperftools/gperftools/releases/download/gperftools-2.2.1/gperftools-2.2.1.tar.gz
tar -zxvf gperftools-2.2.1.tar.g
cd gperftools-2.2.1
./configure
make
make install


   22  sudo apt-get install python-rados
   23  sudo apt-get install librados-dev
   24  ceph -v
   25  sudo apt-get update
   26  sudo apt-get install python-pip
   27  y
   28  yls
   29  ls
   30  sudo pip install PrettyTable












