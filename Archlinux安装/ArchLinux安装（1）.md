首先通过安装介质，启动到live环境。会以root身份进入一个虚拟控制台中，默认的shell是zsh。

### 验证引导模式

`ls /sys/firmware/efi/efivars` 如果结果显示了目录且没有报告错误，则系统是以 UEFI 模式引导的。

### 连接到因特网

#### 判断无线网卡是否被锁

```shell
# rfkill list  
--------------
0: phy0: Wireless LAN
	Soft blocked: yes
	Hard blocked: yes
```

如果出现以上内容，可以调节网卡开关打开它。如果没有开关，那就使用以下命令：

```shell
# rfkill unblock wifi
```

#### 连接网络

```shell
$ iwctl   //会进入联网模式
[iwd]# help    //可以查看帮助
[iwd]# device list    //列出你的无线设备名称，一般以wlan0命名
[iwd]# station <device> scan    //扫描当前环境下的网络
[iwd]# station <device> get-networks    //会显示你扫描到的所有网络
[iwd]# station <device> connect <network name>
password:输入密码
[iwd]# exit    //退出当前模式，回到安装模式
```

测试网络是否连通：

```
# ping baidu.com
```

### 更新系统时间

` timedatectl set-ntp true ` 之后可以使用 ` timedatectl status `检查服务状态

### 建立磁盘分区

