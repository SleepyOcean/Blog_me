# 利用vultr的vps搭建ikev2类型vpn

[TOC]

> 参考链接[CentOS/Ubuntu一键安装IPSEC/IKEV2 VPN服务器](https://quericy.me/blog/699/)

## 服务器端配置

1. 下载一键配置脚本

   ​	脚本[Github链接](https://github.com/quericy/one-key-ikev2-vpn)；

   ```
   wget --no-check-certificate https://raw.githubusercontent.com/quericy/one-key-ikev2-vpn/master/one-key-ikev2.sh
   ```

2. 运行脚本

   ```
   chmod +x one-key-ikev2.sh
   bash one-key-ikev2.sh
   ```

3. 等待自动配置部分内容后，选择vps类型（OpenVZ还是Xen、KVM），**选错将无法成功连接，请务必核实服务器的类型**。输入服务器ip或者绑定的域名(连接vpn时服务器地址将需要与此保持一致,如果是导入泛域名证书这里需要写`*.域名`的形式)；

   vultr的vps类型应该是KVM(不能完全确定，但排除了xen和OpenVZ的可能，具体搜索“如何查看vps类型”)；

4. 选择使用使用证书颁发机构签发的SSL证书还是生成自签名证书：

   - 如果选择no,`使用自签名证书`（客户端如果使用IkeV2方式连接，将需要导入生成的证书并信任）则需要填写证书的相关信息(C,O,CN)，为空将使用默认值(default value)，确认无误后按任意键继续,后续安装过程中会出现输入两次pkcs12证书的密码的提示(可以设置为空)
   - 如果选择yes，`使用SSL证书`（如果证书是被信任的，后续步骤客户端将无需导入证书）请在继续下一步之前，将以下文件按提示命名并放在**脚本相同的目录下**（SSL证书详细配置和自动续期方案可见[https://quericy.me/blog/860/](https://quericy.me/blog/860/) ）：
     1. **ca.cert.pem** 证书颁发机构的CA，比如Let‘s Encrypt的证书,或者其他链证书；
     2. **server.cert.pem** 签发的域名证书；
     3. **server.pem** 签发域名证书时用的私钥；

5. 是否使用SNAT规则(可选).默认为不使用.使用前请确保服务器具有不变的**静态公网ip**,可提升防火墙对数据包的处理速度.如果服务器网络设置了NAT(如AWS的弹性ip机制),则填写网卡连接接口的ip地址(参见[KinonC](https://github.com/KinonC)提供的方案:[#36](https://github.com/quericy/one-key-ikev2-vpn/issues/36))。

6. 防火墙配置.默认配置iptables(如果使用的是firewall(如CentOS7)请选择yes自动配置firewall,将无视SNAT并跳过后续的补充网卡接口步骤).补充网卡接口信息,为空则使用默认值(Xen、KVM默认使用eth0,OpenVZ默认使用venet0).如果服务器使用其他公网接口需要在此指定接口名称,**填写错误VPN连接后将无法访问外网**)。

7. 看到install Complete字样即表示安装完成。默认用户名密码将以黄字显示，可根据提示自行修改配置文件中的用户名密码,多用户则在配置文件中按格式一行一个(多用户时用户名不能使用%any),保存并重启服务生效。

   配置用户信息的文件`/usr/local/etc/ipsec.secrets`

   内容示例如下：

   ```
   : RSA server.pem
   : PSK "myPSKkey"
   : XAUTH "myXAUTHPass"
   test1  : EAP "123456"
   test2  : EAP "123456"
   test3  : XAUTH "123456"
   ```

   上述有test1、test2和test3三个用户，密码都为123456。

   重启服务的命令：`ipsec restart`;

   其他相关命令：

   ```
   ipsec start   #启动服务
   ipsec stop    #关闭服务
   ipsec restart #重启服务
   ipsec reload  #重新读取
   ipsec status  #查看状态
   ipsec --help  #查看帮助
   ```

8. 将提示信息中的证书文件ca.cert.pem拷贝到客户端，修改后缀名为.cer后导入。ios设备使用Ikev1无需导入证书，而是需要在连接时输入共享密钥，共享密钥即是提示信息中的黄字PSK.

   将服务器端的文件下载到本地的方法：

   >1. 如何在 Xshell 终端下载/上传远程服务器的文件
   >
   >   下面以 CentOS 为例： 
   >   1、安装 szrz
   >
   >   ```
   >   yum install lrzsz
   >   ```
   >
   >   2、下载服务器的文件到本地
   >
   >   ```
   >   sz /path/to/file
   >   ```
   >
   >   这时 Xshell 会弹出框框选择文件存放目录  3、上传文件到服务器
   >
   >   ```
   >   rz /path/to/file
   >   ```
   >
   >   这时 Xshell 会弹框选择要上传的文件，然后上传到命令指定的目录下
   >
   >​



## 客户端配置

1. win10 将生成的证书文件ca.cert.pem后缀名改为.cer后，双击导入证书（注意要在导入受信任的根证书颁发机构）；

   Windows10存在些bug,部分Win10系统连接后ip不变,没有自动添加路由表,使用以下方法可解决(本方法由 bigbigfish 童鞋提供):

   - 手动关闭vpn的split tunneling功能(在远程网络上使用默认网关);

   - 也可使用powershell修改,进入CMD窗口,运行如下命令:

     ```
     get-vpnconnection    #检查vpn连接的设置（包括vpn连接的名称）
     set-vpnconnection "vpn连接名称" -splittunneling $false    #关闭split tunneling
     get-vpnconnection   #检查修改结果
     ```

2. android端需要下载[strongSwan VPN Client](https://download.strongswan.org/Android/)，在该客户端下安装证书，配置，连接。



## 服务器端卸载

1. 进入脚本所在目录的strongswan文件夹执行:

   ```
   make uninstall
   ```

2. 删除脚本所在目录的相关文件(one-key-ikev2.sh,strongswan.tar.gz,strongswan文件夹,my_key文件夹)。

3. 卸载后记得检查iptables配置。



## 可能出现的问题

- ipsec启动问题：服务器重启后默认ipsec不会自启动，请命令手动开启,或添加/usr/local/sbin/ipsec start到自启动脚本文件中(如rc.local等)：

  ```
  ipsec start

  ```

- ipsec常用指令:

  ```
  ipsec start   #启动服务
  ipsec stop    #关闭服务
  ipsec restart #重启服务
  ipsec reload  #重新读取
  ipsec status  #查看状态
  ipsec --help  #查看帮助

  ```

- 可连接但是无法访问网络：

  - 检查iptables是否正常启用,检查iptables规则是否与其他地方冲突,或根据服务器防火墙的实际情况手动修改配置。
  - 检查sysctl是否开启ip_forward:
    1. 打开sysctl文件:`vim /etc/sysctl.conf`
    2. 修改net.ipv4.ip_forward=1后保存并关闭文件
    3. 使用以下指令刷新sysctl：`sysctl -p`
    4. 如执行后正常回显则表示生效。如显示错误信息，请重新打开/etc/syctl并根据错误信息对应部分用#号注释，保存后再刷新sysctl直至不会报错为止。

- 如果之前使用的自签名证书，后改用SSL证书，部分客户端可能需要卸载之前安装的自签名证书,否则可能会报`Ike凭证不可接受`的错误:

  - iOS：设置-通用，删除证书对应的描述文件即可；
  - Windows：Win+R,运行mmc打开Microsoft管理控制台,文件->添加管理单元,添加证书管理单元(必须选计算机账户),展开受信任的根证书颁发机构,找到对应的自签名证书,右键删除即可;
  - Windows Phone:暂时没有找到可以卸载证书的方法(除非越狱),目前只能重置来解决此问题;