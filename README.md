# 京东云AX3000路由器CI自动编译固件

本项目自动编译京东云后羿(/哪吒) ，型号RE-CS-03(IPQ5018平台)的固件，OpenWRT官方源码。


|   Snapshots   |   |
| ------------- | ------------- |
|![](images/ScreenShot_2025-11-30_005224_810.png)|![](images/ScreenShot_2025-11-30_005745_918.png)|
|![](images/ScreenShot_2025-11-30_005550_998.png)|![](images/ScreenShot_2025-11-30_005637_027.png)|

## 特点

- [OpenWRT](https://github.com/openwrt/openwrt)官方源码，和设备专用补丁
- 全功能OpenSSH
- 编译全部KMOD安装包，确保安装软件不缺依赖，KMOD安装包在压缩包文件snapshots.tar.gz中
- 发布版本分别为没有NSS加速功能的主线版本和带NSS加速的[Kuncy7](https://github.com/kuncy7/openwrt-nss-edma)仓库版本

## 刷机

刷机需要拆机用TTL刷，详细方法过程参考恩山的[帖子](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=8428102)。

1. 拆机，连接TTL线，网线插到WAN口，配置好终端和TFPTD64，插电
2. TTL终端输入jdqca中断启动过程
3. 刷入uboot，不需要刷大分区
4. 断电后按住JOY插电，web进入刷机界面刷入[Release](https://github.com/pmyy-wt/jdc_re-cs-03/releases/latest)的factory.bin

UBoot刷好之后都可以按上面步骤4进入web刷机界面

固件刷好后，可以在Luci的升级界面刷入sysupgrade.bin进行固件更新

<details>
<summary>有用的UBOOT指令</summary>

- 设置ip
```
setenv serverip 192.168.1.11
setenv ipaddr 192.168.1.1
```
- 启动web刷机
```
httpd 192.168.1.1/24
```
- 备份旧固件
```
mmc dev 0
mmc read 0x44000000 0x0 0x2c622
tftpput 0x44000000 0x58c4400 backup.img
```
- 恢复旧固件
```
dd if=backup.img of=/dev/mmcblk0 bs=512 count=181794
```
或者在uboot
```
mmc dev
tftpboot 0x44000000 backup.img
mmc write 0x44000000 0x0 0x2c622
```
- 刷入uboot
```
setenv serverip 192.168.1.11
setenv ipaddr 192.168.1.1
tftpboot 0x44000000 uboot.mbn
flash 0:APPSBL
```
- 不刷机启动固件
```
setenv serverip 192.168.1.11
setenv ipaddr 192.168.1.2
tftpboot 0x44000000 openwrt-qualcommax-ipq50xx-jdcloud_re-cs-03-initramfs-uImage.itb
bootm 0x44000000
```
</details>

## 鼓励一下

如果你从本项目中受益，欢迎捐赠一杯咖啡 ☕。  
这不仅是对我持续维护的动力，也让开源精神得以延续。

| 微信支付  | 支付宝 |
| ------------- | ------------- |
| ![](images/wx.png)  | ![](images/ali.png) |
