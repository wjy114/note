sudo mkfs.ext4 /dev/pmem12
sudo mount -o dax /dev/pmem12 /mnt/nfs-server 
