# manageQuadTitan
四卡泰坦电脑的说明文档

## 硬件概况

| 条目 | 详情 | 备注 |
| ------ | ------ | ------ |
| 启动项切换 | 开机按F8 | 默认Ubuntu，选择Windows Boot Manager启动Windows系统 |
| 处理器 | Intel E5 2678 V3 x2 |  |
| 主板 | ASUS Z10PE-D8 |  |
| 内存 | Samsung DDR4 ERCC 2400Mhz 16GBx2 | 双通道，由于CPU内存控制器限制实际运行在2133Mhz |
| 硬盘 | Samsung 860 EVO 250GB SATA SSD + WD 2TB 5400rpm HDD | 固态安装有Windows，机械安装有Ubuntu |
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


## 注意
0. 由于服务器共享使用，请不要随意重启服务器以免带来不必要的麻烦。  

1. 如果有必要，使用形如`nohup python -u trian.py &`命令保证进程不间断运行（如断开SSH连接等），`-u`可加可不加，它能保证python程序的输出可以无缓存、及时地更新到nohup.out文件中，对于不会主动关闭的进程，训练后使用`kill`。  
  更多用法参考[其他教程](https://blog.csdn.net/fang_chuan/article/details/82017470)
  
2. 使用`conda`或 `pip`命令来创建和管理**你的环境**。  
  **conda**：使用`conda create -n yourenvname python=pythonversion`命令创建属于你的python环境，例如`conda create -n wtkeras python=2.7`。 创建的环境路径位于`/home/root1root/anaconda3/envs/yourenvname/bin/python`。使用`conda env list`来查看目前存在和激活的的conda环境，使用`source activate yourenvname`来激活你的环境。安装GPU版的tf等框架建议使用pip命令，因为使用conda命令会自动下载对应的cuda和cudnn，不知道时候会有影响。（注：本机当前安装的驱动程序不支持cuda9.2及以上）关于conda、pip命令的更多使用方法，请参考其他教程。 
    
3.  在python中，务必在程序中加入如下代码来指定抢占的GPU，x为0或1或2或3或0,1或0,2等（一共四块，为0123），尽量不占用大量显卡。    
```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = 'x' 
```  
  
  也可以在Terminal中直接在命令前加上`CUDA_VISIBLE_DEVICES=0`来指定GPU  
  
  
4. 在Ubuntu系统中，可以使用如下代码来查看GPU的占用情况, `-n`后指定刷新的时间间隔， `-d`高亮刷新部分，建议运行代码前查看空闲的GPU
```linux
watch -n 1 -d nvidia-smi
```
  
5. 如果使用Pycharm Professional进行远程开发、调试，可以参考[这篇文章](https://blog.csdn.net/yejingtao703/article/details/80292486)（配置时使用自己的Username和环境）  
  并可以在Pycharm Professional的Remote Host中进行图形化文件管理。  
  关于怎样免费获取Pycharm Professional Edition，可在搜索引擎中搜索 **Jetbrain 学生** 或 **Github 学生（推荐）**   ，github学生认证更方便，通过后学生包中直接授权到Jetbrain的学生认证。  
    
6. Windows轻松使用：由于服务器没有安装FTP服务，所以无法在Windows资源管理器中直接添加网络位置，需借助第三方软件。  
  欲在Windows资源管理器中“挂载”该网络位置，在搜索引擎中搜索**SFTP Net Drive（推荐，并在Internet选项中将本服务器IP地址加入本地Intranet来消除安全提示）**或**Swish - Easy SFTP for Windows**了解一下，都基于SSH的子协议SFTP；或使用**WinSCP**来进行图形化的文件管理。  
  下图为Swish的集成效果，可以支持更多本地化操作。  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/SFTP%20Net%20Drive.png)
  
  下图为Swish的集成效果。  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/swish.png)
  
  下图为WinSCP的使用效果  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/winscp.png)  
    
7.如果提示`libcublas.so.9.0: cannot open shared object file: No such file`的话，试一下在自己目录下的`./bashrc` 中末尾添加 `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64/` 然后执行 `source  ~/.bashrc` 一般可以解决。下次运行还会报错，只要执行那条source命令即可，我也不知道为啥。。。。求高人指点
