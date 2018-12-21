# DellOMSA
[![版本](https://img.shields.io/badge/version-6.6.6-brightgreen.svg)](https://github.com/Chuyio/DellOMSA)
[![download](https://img.shields.io/badge/download-20K-yellowgreen.svg)](https://github.com/Chuyio/DellOMSA/archive/master.zip)
[![owner](https://img.shields.io/badge/owner-open%20source-orange.svg)](https://github.com/Chuyio)
[![blogs](https://img.shields.io/badge/blogs-cnlogs-yellow.svg)](https://www.cnblogs.com/LuckWJL/)
[![linkman](https://img.shields.io/badge/linkman-WeChat-green.svg)](http://images.cnblogs.com/cnblogs_com/LuckWJL/988555/o_WeChat.jpg)
[![Wechat Reward](https://img.shields.io/badge/Wechat-Reward-red.svg)](https://www.cnblogs.com/images/cnblogs_com/LuckWJL/988555/o_%e5%be%ae%e4%bf%a1%e8%b5%9e%e8%b5%8f%e7%a0%81.jpg)
[![Alipay Reward](https://img.shields.io/badge/Alipay-Reward-blue.svg)](https://www.cnblogs.com/images/cnblogs_com/LuckWJL/988555/o_%e6%94%af%e4%bb%98%e5%ae%9d%e6%94%b6%e6%ac%be%e7%a0%81.png)

----
----
----

# 安装OMSA
```
git clone git@github.com:Chuyio/DellOMSA.git
bash DellOMSA/bootstrap.sh
yum install -y net-snmp net-snmp-devel net-snmp-utils wget perl OpenIPMI
yum  -y install srvadmin-all                           #安装路径：/opt/dell/srvadmin/
/opt/dell/srvadmin/sbin/srvadmin-services.sh start     #启动OMSA
ln -s /opt/dell/srvadmin/bin/omreport /usr/local/bin/
```

# 官方网址
[![版本](https://img.shields.io/badge/Dell-OpenManage--Server--Administrator-green.svg?logo=appveyor&style=for-the-badge)](https://www.dell.com/support/manuals/cn/zh/cndhs1/dell-opnmang-srvr-admin-v7.4/omsa_cli-v3/omreport-storage-%E5%91%BD%E4%BB%A4?guid=guid-3d33a63a)

# 戴尔服务器使用omreport硬件查看信息

# 一、命令指南
* omreport chassis # 显示所有主要组件的常规状态 
* omreport chassis memory # 显示内存信息
* omreport chassis temps # 显示系统主要组件的温度
* omreport storage adisk controller=0 # 查看磁盘陈列中的硬盘状态
* omreport storage pdisk controller=0 # 查看物理磁盘信息
* omreport storage vdisk controller=0 # 查看虚拟硬盘的状态
* omreport storage controller # 查看控制器(即RAID卡)的属性
* omreport storage enclosure controller=0 # 查看enclosure的属性
* omreport storage battery # 查看电池属性
* 在写脚本的时候还可以这样使用好分割数据：omreport chassis info -fmt ssv

```-fmt ssv```

例：
```
[root@localhost wcy]# omreport chassis info -fmt ssv
Chassis Information

Index;0
Chassis Name;Main System Chassis
Host Name;localhost.localdomain
iDRAC7 Version;1.40.40 (Build 17)
Lifecycle Controller 2 Version;1.1.5.165
Chassis Model;PowerEdge R420
Chassis Lock;Present
Chassis Service Tag;6X8QZY1
Express Service Code;15070774393
Chassis Asset Tag;Unknown
Flash chassis identify LED state;Off
Flash chassis identify LED timeout value;300

[root@localhost wcy]# omreport chassis info
Chassis Information

Index                                      : 0
Chassis Name                               : Main System Chassis
Host Name                                  : localhost.localdomain
iDRAC7 Version                             : 1.40.40 (Build 17)
Lifecycle Controller 2 Version             : 1.1.5.165
Chassis Model                              : PowerEdge R420
Chassis Lock                               : Present
Chassis Service Tag                        : 6X8QZY1
Express Service Code                       : 15070774393
Chassis Asset Tag                          : Unknown
Flash chassis identify LED state           : Off
Flash chassis identify LED timeout value   : 300
```

# 二、命令样例
## 1.系统摘要信息
* 机箱型号＃服务器型号
* 快速服务代码＃快速服务代码（可在在官网查询服务器保修时间）
* 机箱服务标签＃服务标签
```
[root@zeping ~]# omreport chassis info
Chassis Information
Index : 0
Chassis Name : Main System Chassis
Host Name : zeping.linuxhub.cn
iDRAC7 Version : 1.40.40 (Build 17)
Lifecycle Controller 2 Version : 1.1.5.165
Chassis Model : PowerEdge R720
Chassis Lock : Present
Chassis Service Tag : 4TMMGY1
Express Service Code : 10498648393
Chassis Asset Tag : Unknown
Flash chassis identify LED state : Off
Flash chassis identify LED timeout value : 300
```

## 2.显示虚拟磁盘信息（阵列信息）
* 状态＃阵列状态
* 布局＃阵列卡类型
* 尺寸＃阵列空间大小
* 总线协议＃总线协议
* 媒体＃介质
```
[root@zeping ~]# omreport storage vdisk
List of Virtual Disks in the System
Controller PERC H710 Mini (Embedded)
ID : 0
Status : Ok
Name : Virtual Disk0
State : Ready
Hot Spare Policy violated : Not Assigned
Encrypted : No
Layout : RAID-5
Size : 11,176.50 GB (12000675495936 bytes)
T10 Protection Information Status : No
Associated Fluid Cache State : Not Applicable
Device Name : /dev/sda
Bus Protocol : SAS
Media : HDD
Read Policy : Adaptive Read Ahead
Write Policy : Write Back
Cache Policy : Not Applicable
Stripe Element Size : 64 KB
Disk Cache Policy : Disabled
```

## 3.查看物理磁盘信息
* 供应商ID＃供应商ID 
* 产品编号＃磁盘ID 
* 序列号＃序列号
* 部件编号＃部件号
* 修订版＃修订
* 制造日＃磁盘制造日期（日）
* 制造周＃磁盘制造日期（周）
* 制造年＃磁盘制造日期（年份）
* 容量＃磁盘容量
* 已用RAID磁盘空间＃已用RAID磁盘空间
* 可用RAID磁盘空间＃可用RAID磁盘空间
* 扇区大小＃扇区大小
* 热备用＃是否热备份
* 总线协议＃总线协议
* 媒体＃介质
* 协商速度＃协商速度
* 有能力快速＃支持速度
```
[root@zeping ~]# omreport storage controller controller=0 #ID等于0的连接器
ID : 0:1:0
Status : Ok
Name : Physical Disk 0:1:1
State : Online
Power Status : Spun Up
Bus Protocol : SAS
Media : HDD
Part of Cache Pool : Not Applicable
Remaining Rated Write Endurance : Not Applicable
Failure Predicted : No
Revision : MS05
Driver Version : Not Applicable
Model Number : Not Applicable
T10 PI Capable : No
Certified : Yes
Encryption Capable : No
Encrypted : Not Applicable
Progress : Not Applicable
Mirror Set ID : Not Applicable
Capacity : 3,725.50 GB (4000225165312 bytes)
Used RAID Disk Space : 3,725.50 GB (4000225165312 bytes)
Available RAID Disk Space : 0.00 GB (0 bytes)
Hot Spare : No
Vendor ID : DELL(tm)
Product ID : ST4000NM0005
Serial No. : Z4C04CQ7
Part Number : TH0XWM1W2123363I02P9A00
Negotiated Speed : 6.00 Gbps
Capable Speed : 6.00 Gbps
PCIe Negotiated Link Width : Not Applicable
PCIe Maximum Link Width : Not Applicable
Sector Size : 512B
Device Write Cache : Not Applicable
Manufacture Day : 07
Manufacture Week : 11
Manufacture Year : 2016
SAS Address : 5000C500853BD529
Non-RAID HDD Disk Cache Policy : Not Applicable
Disk Cache Policy : Not Applicable
Form Factor : Not Available
Sub Vendor : Not Available
```

## 4.显示系统主要组件的温度
* 读数＃当前温度
* 最小警告阈值＃警告阈值最小温度
* 最大警告阈值＃警告阈值最大温度
* 最小故障阈值＃故障阈值最小温度
* 最大故障阈值＃故障阈值最大温度
```
[root@zeping ~]# omreport chassis temps
Temperature Probes Information
------------------------------------
Main System Chassis Temperatures: Ok
------------------------------------
Index : 0
Status : Ok
Probe Name : System Board Inlet Temp
Reading : 29.0 C
Minimum Warning Threshold : 3.0 C
Maximum Warning Threshold : 42.0 C
Minimum Failure Threshold : -7.0 C
Maximum Failure Threshold : 47.0 C
```
## 5.内存插槽信息
* 已安装容量＃安装容量
* 最大容量＃最大容量
* 操作系统可用的总安装容量＃操作系统可用的总安装容量
* 可用的插槽＃可用的插槽
* 使用的插槽＃已用插槽
* 连接器名称＃连接器上的内存设备
* 类型＃内存类型
* 尺寸＃单条内存大小
```
[root@zeping ~]# omreport chassis memory
Memory Information
Health : Ok
Attributes of Memory Array(s)
Attributes of Memory Array(s)
Location : System Board or Motherboard
Use : System Memory
Installed Capacity : 65536 MB
Maximum Capacity : 786432 MB
Slots Available : 24
Slots Used : 8
Error Correction : Multibit ECC

Total of Memory Array(s)
Total Installed Capacity : 65536 MB
Total Installed Capacity Available to the OS : 64575 MB
Total Maximum Capacity : 786432 MB

Details of Memory Array 1
Index : 0
Status : Ok
Connector Name : DIMM_A1 
Type : DDR3 - Synchronous Registered (Buffered)
Size : 8192 MB
```

## 6.CPU处理器信息
* 处理器品牌＃处理器品牌
* 处理器版本＃处理器版本
* 当前速度＃当前速度
* 核心数＃核心数
```
[root@zeping ~]# omreport chassis processors
Processors Information
Health : Ok
Index : 0
Status : Ok
Connector Name : CPU1
Processor Brand : Intel(R) Xeon(R) CPU E5-2620 0 @ 2.00GHz
Processor Version : Model 45 Stepping 7
Current Speed : 2000 MHz
State : Present
Core Count : 6
```

## 7.CPU处理器详细信息
* 64位支持＃64位支持
* 超线程（HT）＃超线程（HT）
* 虚拟化技术（VT）＃虚拟化技术（VT）
* 基于需求的切换（DBS）＃按需切换技术（DBS）
```
[root@zeping ~]# omreport chassis pwrsupplies
Power Supplies Information
Power Supply Redundancy
Redundancy Status : Full
Individual Power Supply Elements
Index : 0
Status : Ok
Location : PS1 Status
Type : AC
Rated Input Wattage : 900 W
Maximum Output Wattage : 750 W
Firmware Version : 69.45.99
Online Status : Presence Detected
Power Monitoring Capable : Yes
```
## 8.电源设备信息
* 额定输入功率＃额定输入瓦特
* 最大输出功率＃最大输出瓦特
```
[root@zeping ~]# omreport chassis pwrsupplies
Power Supplies Information
Power Supply Redundancy
Redundancy Status : Full
Individual Power Supply Elements
Index : 0
Status : Ok
Location : PS1 Status
Type : AC
Rated Input Wattage : 900 W
Maximum Output Wattage : 750 W
Firmware Version : 69.45.99
Online Status : Presence Detected
Power Monitoring Capable : Yes
```

## 9.电源管理
* 读取＃当前功耗
* 警告阈值＃警告阈值功耗
* 故障阈值＃故障阈值功耗
* Power Headroom＃电源净空
* System Instant Skroom＃系统瞬时净空
* System Peak Headroom＃系统峰值净空
```
[root@zeping ~]# omreport chassis pwrmonitoring
Power Consumption Information

Power Consumption
Index : 2
Status : Ok
Probe Name : System Board Pwr Consumption
Reading : 126 W
Warning Threshold : 896 W
Failure Threshold : 980 W

Amperage
PS1 Current 1 : 0.2 A
PS2 Current 2 : 0.0 A

Power Headroom
System Instantaneous Headroom : 756 W
System Peak Headroom : 613 W

Power Tracking Statistics
Statistic : Energy Consumption
Measurement Start Time : Wed Jul 10 01:37:33 2013
Measurement Finish Time : Fri Jul 8 09:27:41 2016
Reading : 2981.0 kWh

Statistic : System Peak Power
Measurement Start Time : Wed Jul 10 01:37:33 2013
Peak Time : Fri Jul 19 10:32:15 2013
Peak Reading : 287 W

Statistic : System Peak Amperage
Measurement Start Time : Wed Jul 10 01:37:33 2013
Peak Time : Sat Jan 11 07:35:38 2014
Peak Reading : 6.1 A
```

## 10.风扇探测器信息
* 阅读＃当前风扇转速（转/分）
* 最小故障阈值＃故障阈值最小（转/分）
```
[root@zeping ~]# omreport chassis fans
Fan Probes Information

Fan Redundancy
Redundancy Status : Full

Probe List
Index : 0
Status : Ok
Probe Name : System Board Fan1 RPM
Reading : 3960 RPM
Minimum Warning Threshold : 840 RPM
Maximum Warning Threshold : [N/A]
Minimum Failure Threshold : 600 RPM
Maximum Failure Threshold : [N/A]
```

## 11.硬件日志
```
omreport system esmlog #硬件日志
omreport system alertlog #警报日志
omreport system alertaction #查看为系统组件的警告和故障事件所配置的警报措施的摘要
```
# 查看其他信息
```
//内存
# free -m
# cat /proc/meminfo
# dmidecode -t memory
//CPU
# lscpu
# cat /proc/cpuinfo
# dmidecode -t processor
# dmidecode | grep "CPU" //获取CPU信息
//硬盘
# df -lhP
# lsblk
# fdisk -l
# dmesg|grep sd //查看开机信息里面的磁盘info
# hdparm -I /dev/sda //查看磁盘硬件信息、开启的功能等,信息特别详细 【hdparm需要yum安装】
# smartctl -H /dev/sda //查看硬盘健康状态
# smartctl --all /dev/sda //【smartctl需要yum安装才能用】
# smartctl -h //还有很多有用的参数
//网卡
# lspci|grep -i eth
# ifconfig -a
# ip link show
# ethtool eth0 //显示网卡eth0的详细参数和指标
//系统信息
# dmidecode -t system
# dmidecode | grep"Product"
# dmidecode | grep"Manufacturer" //获取厂商
# dmidecode | grep -B 4 "SerialNumber" | more //获取序号信息
# dmidecode | grep "Date" //获取生产日期
//dmesg和dmidecode还有很多信息，涵盖了全部硬件信息。
//主板
# lspci
//BIOS
# dmidecode -t bios
# dmidecode -q //列出所有有用的信息
//RAID信息
# lspci|grep RAID //列出RAID卡的信息
# megacli64 //需要额外安装
```
