yum install centos-release-gluster40.x86_64

服务端：

sudo apt install glusterfs-server

sudo systemctl start glusterd

sudo systemctl enable glusterd

sudo systemctl status glusterd

配置glusterfs-server

以10.0.0.41和10.0.0.42为server进行配置

打开防火墙

sudo iptables -I INPUT -p all -s <ip-address> -j ACCEPT

挂载brick

在10.0.0.41和10.0.0.42上进行挂载

sudo mkdir -p /mnt/glusterfs-brick

sudo mkfs.ext4 /dev/pmem7

sudo mount -o dax /dev/pmem7 /mnt/glusterfs-brick

创建gluster集群

对于10.0.0.41(只需在一台server上进行probe）：

sudo gluster peer probe 10.0.0.42

sudo gluster peer status

sudo gluster pool list

挂载gluster分布式卷

sudo gluster volume create vol01 replica 2 transport tcp \
10.0.0.41:/mnt/glusterfs-brick \
10.0.0.42:/mnt/glusterfs-brick \
force

启动卷：

sudo gluster volume start vol01

查看卷：

sudo gluster volume info vol01

停止卷：

sudo gluster volume stop vol01

删除卷：

sudo gluster volume delete vol01
