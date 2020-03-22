 
###1.deb

```[ceph_deploy][ERROR ] RuntimeError: Failed to execute command: env DEBIAN_FRONTEND=noninteractive DEBIAN_PRIORITY=critical apt-get --assume-yes -q update```

sudo vim /etc/apt/sources.list.d/ceph.list

https://download.ceph.com/debian-{ceph-stable-release}

to

https://download.ceph.com/debian-luminous

###2.hostname

connecting to host: ddst-PowerEdge-R8204 resulted in errors: HostNotFound 
