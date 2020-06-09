
sudo
``echo 0 > /proc/sys/kernel/randomize_va_space``

filebench -f fileserver.f

sudo numactl --physcpubind=+18-35,54-71 fio fio-read-1

