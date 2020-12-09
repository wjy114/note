额外查看cmake 版本3.3加
sudo ./bootstrap --prefix=/usr/local

sudo make

sudo make install

vi ~/.bash_profile   PATH=/usr/local/bin:$PATH:$HOME/bin

cmake --version

添加mpich文件及查看java路径

sudo yum install fuse-devel cryptopp-devel boost-devel libibverbs-devel gcc-c++ openssl-devel

在/etc/profile

/home/jingyu/.bashrc

/home/jingyu/.bash_profile

中添加以下

'''
export PATH=/home/jingyu/mpich/bin:$PATH
export PATH=/usr/local/bin:$PATH
export INCLUDE=/home/jingyu/mpich/include:$INCLUDE
export LD_LIBRARY_PATH=/home/jingyu/mpich/lib:$LD_LIBRARY_PATH
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.272.b10-1.el7_9.x86_64
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
'''

source /etc/profile

source /home/jingyu/.bashrc

source /home/jingyu/.bash_profile

更新
sudo which mpicc看一下是否成功

在/etc/sudoers中添加：

Defaults secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin:/home/jingyu/mpich/bin:/usr/local/bin:/home/jingyu/mpich/include:/home/jingyu/mpich/lib:/usr/lib64/mpich/bin:/usr/include/mpich-x86_64:/usr/lib64/mpich/lib:/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.272.b10-1.el7_9.x86_64

net/memZZZ 文件中修改pmem路径
xml中只写管理节点 并按顺序执行
管理节点

sudo mkdir build

cd build

sudo make clean

sudo cmake ..

sudo make -j8

sudo ./dmfs

客户端
sudo cd build

sudo make clean

sudo cmake ..

sudo make -j8

sudo ./fusenrfs -f -o direct_io /home/jingyu/mnt
