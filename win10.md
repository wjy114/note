### WIN10 企业版 LTSC 

按下组合键Windows + R以打开运行窗口。输入powershell然后按下回车键。

Windows PowerShell会以当前用户的权限去执行。

如果你想要从普通模式转至管理员模式，输入以下PowerShell命令然后按下回车键。

```
Start-Process powershell -Verb runAs
```

以管理员权限输入：

```
slmgr -ipk M7XTQ-FN8P6-TTKYV-9D4CC-J462D
slmgr -skms kms.03k.org
slmgr -ato
slmgr -dlv
```

位置：
```
D:\迅雷下载\cn_windows_10_enterprise_ltsc_2019_x64_dvd_9c09ff24.iso
```
