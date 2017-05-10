# [Vultr搭建ShadowSocks教程](http://www.07net01.com/2017/03/1821867.html)



*本文原创来自：http://www.jianshu.com/p/4bf38b9d5d78*

## 实现原理

本地发起连接请求，由远程[服务器](http://www.07net01.com/tags-%E6%9C%8D%E5%8A%A1%E5%99%A8-0.html)响应后然后将我们需要的数据返回到本地。

最低消费

  2.5美元/月

## 网速自测

经过我个人测试后选择的Dallas节点，浏览[YouTube](http://www.07net01.com/tags-YouTube-0.html)视频，网速能达到1MB/s
，玩美服lol的延迟是200ms-250ms，这个速度已经很不错了，具体分析参考《国内连接[美国](http://www.wredian.com/tags-%E7%BE%8E%E5%9B%BD-0.html)VPN延迟(PING)多少算正常？》。

PS：浏览其他人的[博客](http://www.07net01.com/tags-%E5%8D%9A%E5%AE%A2-0.html)都推荐的是LosAngeles节点，具体的自己通过测试后拿主意吧，懒得测试就选[洛杉矶](http://www.wredian.com/tags-%E6%B4%9B%E6%9D%89%E7%9F%B6-0.html)节点吧。

了解更多，请戳Vultr 节点哪个比较快？

## [知识](http://www.wredian.com/zhishi/)储备

1. 懂[linux](http://www.07net01.com/linux/)最好，不懂就按照下面操作来吧
2. 肯折腾
3. 懂英语，不懂的话…搭建SS（Shadow Socks）干吗？

## 具体步骤

### 购买服务器

1. 打开链接1：我的夏季促销推广链接，无效的话尝试 链接2：我的普通推广链接。
   这两个都指向官网，信不过我的自己去[百度](http://www.07net01.com/tags-%E7%99%BE%E5%BA%A6-0.html)搜索vultr官网。【更多信息见最后的Vlutr服务器链接详细说明】
2. 注册账号并验证邮箱。
3. 测试速度或直接选择洛杉矶节点，测试节点网速请戳我并拉到页面最下面。如果感觉不满意，去试试其他的服务器提供商比如搬瓦工等等，个人感觉vultr还可以。
4. 充值，点击左侧的Billing，最低五美元，这一步因人而异。我个人选择使用Paypal支付的$5。
5. 搭建服务器，点击左侧的Servers，依次选择Server Location——你测试的最快的或者洛杉矶；Server Type——Cent OS7 x64（这个我可以提供[技术](http://www.07net01.com/tags-%E6%8A%80%E6%9C%AF-0.html)支持，本文基于CentOS 7 x64）或其他你懂的；Server Size——只是搭建ss，选第一个就够了($5/mon)；其他的选填。然后点击右下角的Deploy Now。稍等片刻，服务器就可以装好了。
6. 装好后，你可以看到如下界面：

![img](http://img4.07net01.com/upload/images/2017/03/04/36376041422501.png)

servers

点击可以查看服务器的相关信息：

![img](http://img4.07net01.com/upload/images/2017/03/04/36376041422502.png)

server information

接下来操作需要的信息是IP Address，Username和Password。这个页面不要关，一会直接复制粘贴相关信息。

### 远程连接服务器

> 方法①点击刚才的[网页](http://www.07net01.com/tags-%E7%BD%91%E9%A1%B5-0.html)的右上角的五个按钮最左边的View Console进行操作。
>
> 方法②下载Xshell进行操作，建议后者，方面使用（主要是支持复制粘贴）管理。

以Xshell为例。

1. 下载安装Xshell。
2. 安装完成后新建会话（Alt+N）。依次填写图中信息。
   名称可以是Vultr或者其他，协议选择SSH，主机填写之前的IP Address，端口号选择22。

![img](http://img4.07net01.com/upload/images/2017/03/04/36376041422503.png)

连接

点击左侧的用户身份验证，填写信息。方法选择Password，用户名为之前的Username（一般都是root），密码为之前的Password（这个建议直接复制粘贴过来，系统给的有点复杂）

![img](http://img4.07net01.com/upload/images/2017/03/04/36376041422504.png)

用户身份验证

填写完之后点击确定。然后点击连接。出现其他提示的话选择接受就可以了。这时你就可以看到一个命令控制台了。这时就算连接成功了。

### 搭建 Shadowsocks 服务

#### 安装组件

```
$ yum install m2crypto python-setuptools
$ easy_install pip
$ pip install shadowsocks
```

#### 安装完成后配置服务器参数

```
$ vi  /etc/shadowsocks.json
```

写入如下配置:

```
{
    "server":"0.0.0.0",
    "server_port":443,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"123456",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

多端口的如下：

```
{
    "server":"0.0.0.0",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password": {
         "443": "443",
         "8888": "8888"
     },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

其中server字段与local_address填写之前的IP Address。password是自己用于连接这个shadow socks的密码，自定义就好。
其他的不需要更改。

然后保存退出。

vi 的命令: 按 "i" 进入编辑模式，编辑后按 "esc" 退出编辑模式， 输入 ":wq" 保存退出vi。

### 配置[防火墙](http://www.07net01.com/tags-%E9%98%B2%E7%81%AB%E5%A2%99-0.html)

这一步主要是为了提高系统[安全](http://www.07net01.com/security/)性。

```
# 安装防火墙
$ yum install firewalld
# 启动防火墙
$ systemctl start firewalld
```

#### 开启防火墙相应的端口

方法一(推荐)

```
# 端口号是你自己设置的端口
$ firewall-cmd --permanent --zone=public --add-port=443/tcp
$ firewall-cmd --reload
```

方法二（麻烦，没必要）

新建文件ss.xml

```
$ vi /usr/lib/firewalld/services/ss.xml
```

粘贴下面的代码

```
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>SS</short>
  <description>Shadowsocks port
  </description>
  <port protocol="tcp" port="443"/>
</service>
```

保存退出。

开启端口，重启firewalld 服务，下面的ss是上述的文件的名字，区分大小写

```
$ firewall-cmd --permanent --add-service=ss
$ firewall-cmd --reload
```

### 启动 Shadowsocks 服务

```
$ ssserver -c /etc/shadowsocks.json
```

如果想干点其他的实现后台运行，使用

```
$ nohup ssserver -c /etc/shadowsocks.json & （或者ssserver -c /etc/shadowsocks.json -d start）
```

## 连接

这样服务器就搭建好了。全平台的连接方法戳我。

### PC连接

下载Shadow Socks[客户端](http://www.07net01.com/tags-%E5%AE%A2%E6%88%B7%E7%AB%AF-0.html)。SS加速器客户端下载
选择适合的版本，下载并解压运行。

填写信息:服务器地址，端口号，密码，加密方式与代理端口默认即可

![img](http://img4.07net01.com/upload/images/2017/03/04/36376041422505.png)

SS信息填写

填写完之后点击确定，然后到托盘中右键选择开启"启用系统代理"。

### iOS连接

在App Store下载Wingy。

填写信息:服务器，端口，密码，代理模式，加密方式默认即可。

![img](http://img4.07net01.com/upload/images/2017/03/04/36376041422506.png)

Wingy信息填写

### MacOS连接

官方教程

### [android](http://www.07net01.com/tags-android-0.html)连接

官方教程

## 国外站点

[Google](http://www.07net01.com/tags-Google-0.html)

Youtube

[Facebook](http://www.07net01.com/tags-Facebook-0.html)

如果以上没有问题的话，这时候你就可以畅游外面的世界了。点击上述链接测试吧。

##  

 

# Vultr CentOS7安装Google BBR加速工具方法

*本文原创来自：https://www.vultrclub.com/174.html*

一般而言，服务器本身的速度是决定我们[项目](http://www.07net01.com/tags-%E9%A1%B9%E7%9B%AE-0.html)打开速度、下载速度的关键，但是我们也可以借助第三方[软件](http://www.07net01.com/ruanjiantuijian/)工具等提高加速效果，比如我们肯定很多人都熟悉的锐速、Net-Speeder可以双倍发包流量，可以减少超时和提高下载速度。这不在前一段时间，来自大名鼎鼎的[谷歌](http://www.07net01.com/tags-%E8%B0%B7%E6%AD%8C-0.html)发布开源Google BBR工具，可以提高发包数据量，起到加速作用。

这里，我们也在Vultr VPS中安装Google BBR工具，因为是支持KVM和XEN架构的，我们的VULTR都是KVM架构所以肯定支持，但是由于内核的问题，我们需要调试和安装必备的内核和组件才 可以使用，我们一起安装试试吧。

第一、准备工作

这里我选择使用Vultr美国洛杉矶机房5美金月付方案，系统采用CENTOS7 64BIT。很多人要问为什么不选择速度较好的[日本](http://www.wredian.com/tags-%E6%97%A5%E6%9C%AC-0.html)机房，因为日本机房虽然目前用NTT线路，PING速度看着还可以，但是[稳定性](http://www.qiche887.com/tags-%E7%A8%B3%E5%AE%9A%E6%80%A7-0.html)不行，所以我不选择，尤其是晚上速度很差。

第二、查看当前核心

uname -r

这里我们看到当前CENTOS7核心是3.10.0-514.2.2.el7.x86_64，这个核心是不可以安装BBR的。

第三、更新内核

rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm

安装4.9.0内核

yum --enablerepo=elrepo-kernel install kernel-ml -y

我们要知道，BBR目前只支持4.9.0内核，其他内核是不行的，需要更换内核才可以。

第四、检查内核是否更新

rpm -qa | grep kernel

我们看到了有4.9.0内核，需要启动才可以。

grub2-set-default 1

然后重启

shutdown -r now

第五、检查是否生效

uname -r

检查当前内核是不是4.9.4-1.el7.elrepo.x86_64.

看来内核是搞定了，我们那就开始安装BBR了。

第六、安装Google BBR

echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf

echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf

sysctl -p

第七、检查BBR是否成功

sysctl net.ipv4.tcp_available_congestion_control

执行命令，看看是否是提示"net.ipv4.tcp_available_congestion_control = bbr cubic reno"

sysctl -n net.ipv4.tcp_congestion_control

执行命令，是否提示bbr

lsmod | grep bbr

执行命令，是否看到BBR提示。

能看到上面提示，就说明BBR安装成功。后面，我们再去安装需要的工具，比如SS或者其他项目，速度上是有明显提升的。