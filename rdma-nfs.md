## NFS over RDMA

### server:
mkdir /mnt/nfs-server 

sudo mkfs.ext4 /dev/pmem12

sudo mount -o dax /dev/pmem12 /mnt/nfs-server 

vim /etc/exports 添加
 
/mnt/nfs-server *(rw,async,insecure,no_root_squash)

modprobe svcrdma

service nfs start

echo rdma 20049 > /proc/fs/nfsd/portlist

### client

mkdir /mnt/nfs

service nfs start

modprobe svcrdma

service nfs start

echo rdma 20049 > /proc/fs/nfsd/portlist

mount -o rdma,port=20049 10.0.0.50:/mnt/nfs-server /mnt/nfs
