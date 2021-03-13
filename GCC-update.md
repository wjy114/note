## gcc升级

1、安装centos-release-scl

sudo yum install centos-release-scl
2、安装devtoolset，注意，如果想安装7.*版本的，就改成devtoolset-7-gcc*，以此类推

sudo yum install devtoolset-8-gcc*
3、激活对应的devtoolset，所以你可以一次安装多个版本的devtoolset，需要的时候用下面这条命令切换到对应的版本

scl enable devtoolset-8 bash
大功告成，查看一下gcc版本

gcc -v
显示为 gcc version 8.3.1 20190311 (Red Hat 8.3.1-3) (GCC)

sudo yum install devtoolset-7
想要访问 GCC 7，你需要使用软件集合工具scl，启动一个新的 shell：

scl enable devtoolset-7 bash
