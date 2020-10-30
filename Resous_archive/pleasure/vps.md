# 如何开心愉快

## 免费资源 OR 收费资源
网上有很多免费的科学上网工具，但一般带宽设限使用时相当慢，其次则是客户端广告较多影响体验。因此比较推荐购买 VPS 主机资源进行服务器搭建。

国内市占率较高的主要有两家，分别是 ***搬瓦工*** 与 ***Vultr*** ，两者之间的主要区别在于：前者是按年或月收费，后者则按照使用时长收费。

**购买链接：**
* [搬瓦工购买链接](https://bandwagonhost.com/aff.php?aff=58847)
* [Vultr购买链接](https://www.vultr.com/?ref=8348472)

## 服务器远程连接工具
请下载[《电脑装机软件合集》](../software_pc.html)里的 ***SecureCRT远程连接软件***，安装后使用。

## 安装搭建脚本
使用Secure CRT工具连接成功后，粘贴下面的命令进行操作：
```
[root@vultr ~]# yum install -y wget && wget --no-check-certificate -O shadowsocks-libev.sh https://raw.githubusercontent.com/uxh/shadowsocks_bash/master/shadowsocks-libev.sh && bash shadowsocks-libev.sh
```

回车后系统会自行下载脚本文件并运行，按照下图提示，我们输入1选择安装服务，回车继续：
```
2020-01-12 15:44:00 (51.7 MB/s) - ‘shadowsocks-libev.sh’ saved [21414/21414]
==============================================
 Shadowsocks Server Management Script (libev) 
==============================================
 1. Shadowsocks Server Install                
 2. Shadowsocks Server Uninstall              
 3. Shadowsocks Server Update                 
----------------------------------------------
 4. Shadowsocks Server Start                  
 5. Shadowsocks Server Stop                   
 6. Shadowsocks Server Restart                
----------------------------------------------
 7. Shadowsocks Config Status                 
 8. Shadowsocks Config Modify                 
==============================================
Not installed

Please Enter the Number:1

```

回车后系统会进入安装界面，我们首先依次输入 SS 的各项信息，然后回车继续即可：
```
[Info] Start set shadowsocks's config information...
[Info] Wherever you are not sure, just press Enter to continue.

Please enter shadowsocks's password
[Default is 123456]:
-------------------------------
Shadowsocks's Password: 123456
-------------------------------
Please enter shadowsocks's port (1~65535)
[Default is 33526]:
-------------------------------
Shadowsocks's Port: 33526
-------------------------------
Please select shadowsocks's stream cipher
1) aes-256-gcm
2) aes-256-ctr
3) aes-256-cfb
4) chacha20-ietf-poly1305
5) chacha20-ietf
6) chacha20
7) rc4-md5
[Default is aes-256-gcm]:3
-------------------------------
Shadowsocks's Streamcipher: aes-256-cfb
-------------------------------

Press Enter to continue...or Press Ctrl+C to cancel
```

安装过程耗时 2~5 分钟，完成后会来到下图界面：
```
[Info] Congratulations, Shadowsocks has been installed successfully.
=================================================
Server IP        :  1.1.1.1 
Server Port      :  33526 
Password         :  123456 
Encryption Method:  aes-256-cfb 
-------------------------------------------------
ss://YWVzLTI1Ni1jZmI6TnVtYmVyMTQzMzIyM0AxNDkuMjguMTMyLjEzMzo5NTI2
=================================================
You can find the config's backup in /root/shadowsocks.txt.

For more tutorials: https://www.banwagongzw.com & https://www.vultrcn.com

```

接下来需要安装锐速TCP加速软件，由于系统自带内核版本太高无法安装锐速，需要进行降级，复制命令进行操作：
```
[root@vultr ~]# wget --no-check-certificate -O rskernel.sh https://raw.githubusercontent.com/uxh/shadowsocks_bash/master/rskernel.sh && bash rskernel.sh
```

回车后系统会自动下载脚本并执行更换内核命令，按图提示，我们可以看到当前系统为CentOS7，等待内核更换完毕后系统会自动重启并断开连接：
```
[INFO] System OS is CentOS7. Processing...
-------------------------------------------
Retrieving https://filedown.me/Linux/Kernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:kernel-3.10.0-229.1.2.el7        ################################# [100%]
-------------------------------------------
[INFO] Success! Your server will reboot in 3s...
[INFO] Success! Your server will reboot in 2s...
[INFO] Success! Your server will reboot in 1s...
[INFO] Reboot...
```

系统重启后，软件会断开连接，等待3分钟左右服务器即可重启完毕，我们重新连接服务器，按图提示，我们继续复制命令：
```
// [root@vultr ~]# yum install net-tools -y && wget --no-check-certificate -O appex.sh https://raw.githubusercontent.com/0oVicero0/serverSpeeder_Install/master/appex.sh && bash appex.sh install

wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh
```

回车后系统会自动下载脚本并执行，按图提示，我们直接回车继续即可：
```
                      Local Time :   2020-01-12 [16:00:09]       
            ======================================================
            |                    serverSpeeder                   |
            |                                         for Linux  |
            |----------------------------------------------------|
            |                                       -- By .Vicer |
            ======================================================

Preparatory work...
Press Enter to Continue...
Preparatory work...
Press Enter to Continue...
Archive:  /tmp/appex.zip
  inflating: /tmp/appex/install.sh   
   creating: /tmp/appex/apxfiles/
   creating: /tmp/appex/apxfiles/bin/
  inflating: /tmp/appex/apxfiles/bin/renewLic.sh  
  inflating: /tmp/appex/apxfiles/bin/serverSpeeder.sh  
  inflating: /tmp/appex/apxfiles/bin/setConfig.sh  
  inflating: /tmp/appex/apxfiles/bin/showConfig.sh  
  inflating: /tmp/appex/apxfiles/bin/update.sh  
  inflating: /tmp/appex/apxfiles/bin/utils.sh  
   creating: /tmp/appex/apxfiles/etc/
  inflating: /tmp/appex/apxfiles/etc/config  
Lic generate success! 
Installation done!
```

回车继续后系统会自动安装锐速，同时会先后要求我们设置锐速的三项信息，按图提示，我们每次都直接回车继续即可：
```
----
You are about to be asked to enter information that will be used by ServerSpeeder,
there are several fields and you can leave them blank,
for all fields there will be a default value.
----
Accelerate VPN (PPTP,L2TP,etc.)? [n]:
Auto load ServerSpeeder on linux start-up? [y]:
/etc/centos-release:CentOS Linux release 7.7.1908 (Core)
/etc/os-release:NAME="CentOS Linux"
/etc/os-release:PRETTY_NAME="CentOS Linux 7 (Core)"
/etc/os-release:CENTOS_MANTISBT_PROJECT="CentOS-7"
/etc/redhat-release:CentOS Linux release 7.7.1908 (Core)
/etc/system-release:CentOS Linux release 7.7.1908 (Core)
Run ServerSpeeder now? [y]:
```

设置完三项信息完成后，系统会完成锐速安装并输出锐速的运行状态，按图提示，当出现红框内信息时说明锐速已完成安装并开机自启动：
```
(license 628A71EDC5706E97151885d3)
[Running Status]
ServerSpeeder is running!
version              3.11.20.10

[License Information]
License              628A71EDC5706E97 (valid on current device)
MaxSession           unlimited
MaxTcpAccSession     unlimited
MaxBandwidth(kbps)   1024000
ExpireDate           2035-12-31
```

在使用的过程中如果需要修改相关的配置信息，请使用下面的命令：

附一、修改Shadowsocks的配置信息
> 如果你以后需要修改Shadowsocks的配置（比如密码、端口或者加密），可以运行下列命令：
> * 中文版：bash shadowsocks-libev_CN.sh
> * 英文版：bash shadowsocks-libev.sh
> * 然后选择第 8 项：修改Shadowsocks配置即可重新设置Shadowsocks的密码、端口以及加密方式。

附二、卸载Shadowsocks服务
> 如果你以后需要卸载Shadowsocks服务，可以运行下列命令：
> * 中文版：bash shadowsocks-libev_CN.sh
> * 英文版：bash shadowsocks-libev.sh
> * 然后选择第 2 项：卸载Shadowsocks服务即可从服务器中卸载掉Shadowsocks服务。

## 小飞机下载
安装完成后，需要在我们需要科学上网的设备上安装相应软件来连接，下载请参考[《电脑装机软件合集》](../software_pc.html)里的 ***Shadowsocks小飞机***，安装后使用。
