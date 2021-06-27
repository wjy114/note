
### linux shell 操作

https://zhuanlan.zhihu.com/p/67943584 

查找包含指定关键字的文件

$ grep -rn "int main(void)" $

指定规则文件进行搜索

$ cat filename |grep -f key.txt $

### git加密钥

https://blog.csdn.net/YanceChen2013/article/details/82218356

ssh-keygen -t rsa -C “wjy@qq.com”

ssh -T git@github.com
