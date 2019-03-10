# manageQuadTitan
四卡泰坦电脑的说明文档

## 硬件概况

| 条目 | 详情 | 备注 |
| ------ | ------ | ------ |
| 启动项切换 | 开机按F8 | 默认Ubuntu，选择Windows Boot Manager启动Windows系统 |
| 处理器 | Intel E5 2678 V3 x2 |  |
| 主板 | ASUS Z10PE-D8 |  |
| 内存 | Samsung DDR4 ERCC 2400Mhz 16GBx2 | 双通道，由于CPU内存控制器限制实际运行在2133Mhz |
| 硬盘 | Samsung 860 EVO 250GB SATA SSD + WD 2TB 5400rpm HDD | 固态安装有Windwos，机械安装有Ubuntu |
| 显卡 | Nvidia Titan Xp 12GB x4 |  |

## Ubuntu系统（机械硬盘）

### 环境
Ubuntu：16.04.4  
Nvidia Driver：384.130  
CUDA：9.0  
cuDNN：cuDNN v7.4.2 (Dec 14, 2018), for CUDA 9.0  
IP：10.21.6.96  

### root 账户（不推荐）
Username：root1root  
Password：112233

### Teamviewer（不推荐）
ID：1199518541  
PW：qweasdzxc

### 个人的账户和密码（推荐通过SSH登录）
Username：姓名首字母缩写（小写）  
Password：姓名全拼（小写）  
请尽快修改密码  

| 姓名 | 用户名 | 密码 | 组 | 初始路径 | sudo权限  |
| ------ | ------ | ------ | ------ | ------ | ------ |
| 华璟 | hj | huajing | teacher | /home/hj | 是 |
| 王慧燕 | why | wanghuiyan | teacher | /home/why | 是 |
| 徐扬 | xy | xuyang | student | /home/xy | 是 |
| 雷蕾 | ll | leilei | student | /home/ll | 是 |
| 潘峥昊 | pzh | panzhenghao | student | /home/pzh | 是 |
| 王腾 | wt | wangteng | student | /home/wt | 是 |
| 陶家威 | tjw | taojiawei | student | /home/tjw | 是 |
| 陈海英 | chy | chenhaiying | student | /home/chy | 是 |
| 高明琦 | gmq | gaomingqi | student | /home/gmq | 是 |
| 罗利鹏 | llp | luolipeng | student | /home/llp | 是 |

## Windows系统（固态硬盘）

### 环境
Windows：Windows10 1809  
Nvidia Driver：未核实  
CUDA：无  
cuDNN：无  
IP：10.21.6.96  

### 账户
用户名：root1root  
密码：无

### Teamviewer（不推荐）
ID：1202299635  
PW：qweasdzxc

## 注
1. 在python中，务必使用如下代码来指定抢占的GPU，x为0或1或2或3
```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = 'x' 
```
2. 在Ubuntu系统中，可以使用如下代码来查看GPU的占用情况, `-n`后指定刷新的时间间隔， `-d`高亮刷新部分
```linux
watch -n 1 -d nvidia-smi
```
3.Teamviewer已被某用户由14降级到13。对服务器配置进行变更时请及时提交Pull Request或通知本人更改。
