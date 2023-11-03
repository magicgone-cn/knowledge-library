# 为Win10-UWP应用使用设置代理

## 问题描述 

win10的沙箱机制，导致应用商店安装的应用，无法连接localhost网络，当使用代理时，无法访问本地代理客户端

## 解决方案

使用CheckNetIsolation LoopbackExempt命令解除限制

> CheckNetIsolation.exe LoopbackExempt -a -n="Microsoft.Office.OneNote_8wekyb3d8bbwe"

```
CheckNetIsolation LoopbackExempt [operation] [-n=] [-p=]
      操作列表:
          -a  -  向环回免除列表中添加 AppContainer 或程序包系列。
          -d  -  从环回免除列表中删除 AppContainer 或程序包系列。
          -c  -  清除环回免除的 AppContainer 和程序包系列的列表。
          -s  -  显示环回免除的 AppContainer 和程序包系列的列表。

      参数列表:
          -n= - AppContainer 名称或程序包系列名称。
          -p= - AppContainer 或程序包系列安全标识符(SID)。
          -?  - 显示 LoopbackExempt 模块的此帮助消息。
```
> AppContainer 名称获取方法

在文件夹中找到对应程序的名称 
C:\Users\%username%\AppData\Local\Packages

