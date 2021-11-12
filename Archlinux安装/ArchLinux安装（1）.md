首先通过安装介质启动到live环境。选第一个

![](boot选项.png)

会以root身份进入一个虚拟控制台中，默认的shell是zsh。

### 验证引导模式

`ls /sys/firmware/efi/efivars` 如果结果显示了目录且没有报告错误，则系统是以 UEFI 模式引导的。

![](启动方式确认.png)

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

### 更新为国内镜像源

`reflector --country China --age 72 --sort rate --protocol https --save /etc/pacman.d/mirrorlist`

已将最新的镜像源更新为国内的，保存在/etc/pacman.d/mirrorlist目录下

### 磁盘分区

安装archlinux所需分区

```
EFI分区		300 MB
swap分区		4GB
root分区		剩余空间
```

使用cfdisk工具分区 `cfdisk <install disk name >` 比如我的： `cfdisk /dev/sda` 。之后会进入如下界面，选择gpt分区表：

![](分区表类型.png)

之后就开始正式分区了，首先EFI分区，点击new新建：

![](新建分区new.png)

这里输入300M，之后回车，就回到上面的界面了。

![](C:\Users\klelee\Desktop\klnote\Archlinux安装\EFI分区300M.png)

在建立下一个分区之前，先对第一个EFI分区的类型做一个修改，选择type选项

![](./更改EFI分区类型.png)

重复之前的步骤，建立swap分区和root分区，完成之后如下图：

![](./分完区之后.png)

### 格式化分区

#### EFI分区格式化

```
mkfs.vfat /dev/sda1
```

#### root分区格式化

```
mkfs.xfs /dev/sda3
```

#### 创建swap分区

```
mkswap /dev/sda2
```

可以使用`lsblk -f`  查看磁盘分区情况

### 挂在分区

```
mount /dev/sda3 /mnt
mkdir -p /mnt/boot/efi
mount /dev/vda1 /mnt/boot/efi
swapon /dev/sda2
lsblk -f    ## 查看分区g情况 
```

### 安装系统

```
pacstrap /mnt linux linux-firmware linux-headers base base-devel vim git bash-completion
```

### 生成文件系统的表文件

```
genfstab -U /mnt
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab
```

### 进入新系统

```
arch-chroot /mnt
```

### 更新数据库

```
pacman -Syy
```

### 安装相关包

```
pacman -S grub efibootmgr efivar networkmanager intel-ucode
```

### 配置grub

```
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

### 激活启用NetworkManager

```
systemctl enable NetworkManager
```

### 给root用户建立密码

```
passwd
输入密码
```

## 重启

```
exit

umount /mnt/boot/efi
umount /mnt

reboot
```

## 基本配置



