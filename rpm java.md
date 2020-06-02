首先安装alien，

apt-get install alien

使用alien将rpm包转换成.deb格式的包

alien package.rpm

执行完成后生成一个.deb的软件包,再通过dpkg安装.deb格式的包

dpkg -i package.deb

还有一种方法,直接使用alien安装rpm格式的包,自己还没有试过.

alien -i package.rpm
/usr/java/jdk1.7.0_04/bin/javac Hello.java
