# manageQuadTitan
四卡泰坦电脑1的说明文档

## 硬件概况

| 条目 | 详情 | 备注 |
| ------ | ------ | ------ |
| 启动项切换 | 开机按F8 | 默认Ubuntu，选择Windows Boot Manager启动Windows系统，基本不用 |
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

### 公共账户（不推荐）
Username：root1root  
Password：112233


### 个人的账户和密码（通过SSH登录）
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


## 其他提醒
### 警告
由于服务器共享使用，请不要随意重启服务器以免带来不必要的麻烦。  
### 【必读】指定计算使用哪块GPU    
运行程序前，务必使用如下命令来查看GPU的空闲情况, `-n`后指定刷新的时间间隔， `-d`高亮刷新部分。  
```linux
watch -n 1 -d nvidia-smi
```
例如在python中，在运行前，务必在程序中加入如下代码来指定抢占的GPU，x为0或1或2或3或0,1或0,2等（一共四块，为0123），同一块显卡可以运行多个程序，但是可能在某时刻一方程序会因为显存不足退出。如没有必要，不一次占用2块以上显卡。    
```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = 'x' 
```  
也可以直接在终端的python命令前加上`CUDA_VISIBLE_DEVICES=0`来指定GPU。  
### 不间断运行python程序
使用形如`nohup python -u trian.py &`命令保证进程不间断运行（如断开SSH连接等），`-u`可加可不加，它能保证python程序的输出可以无缓存、及时地更新到nohup.out文件中，对于不会主动停止的进程，训练后使用`kill`。  
使用`nohup python -u trian.py > customfileout.txt 2>&1 &`形如这样的命令来指定输出文件名，2、1是标准输出流和错误流。  
更多用法参考[其他教程](https://blog.csdn.net/fang_chuan/article/details/82017470)  
### 使用conda创建和管理环境
使用`conda`或 `pip`命令来创建和管理**你的环境**。  
**conda**：使用`conda create -n yourenvname python=pythonversion`命令创建属于你的python环境，例如`conda create -n wtkeras python=2.7`。 创建的环境路径可通过`conda env list`命令查看。  
使用`conda env list`来查看目前存在和激活的的conda环境，使用`source activate yourenvname`来激活你的环境。安装GPU版的tf等框架建议使用pip命令，因为使用conda命令会自动下载对应的cuda和cudnn。关于conda、pip命令的更多使用方法，请参考其他教程。  
注意：本机当前安装的驱动程序及系统版本限制，不支持cuda9.2及以上版本。 
### 使用Pycharm在远程环境中运行
如果使用Pycharm Professional进行远程开发、调试，可以参考[这篇文章](https://blog.csdn.net/yejingtao703/article/details/80292486)（配置时使用自己的Username和环境）并可以在Pycharm Professional的Remote Host中进行图形化文件管理。  
关于怎样免费使用Pycharm Professional，可在搜索引擎中搜索 **Jetbrain 学生** 或 **Github 学生（推荐）**   ，github学生认证更方便，通过后学生包中直接授权到Jetbrain的学生认证。   
  
如果提示`libcublas.so.9.0: cannot open shared object file: No such file`的话，试一下在自己目录下的`./bashrc` 中末尾添加 `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64/` 然后执行 `source  ~/.bashrc` 一般可以解决。下次运行还会报错，只要执行那条source命令即可，我也不知道为啥。。。。求高人指点。在Pycharm中运行时，将 `LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64/`添加到运行配置的环境变量中即可 。另外，有一位Mac用户需要在Pycharm的configuration中的环境变量中加上LD_LIBRARY_PATH，使用mac的同学注意一下。  
使用nvcc使用下面三行命令
```linux
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64
export PATH=$PATH:/usr/local/cuda-9.0/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-9.0
```
### 使用Clion在远程环境中运行
Clion远程配置[官方教程](https://www.jetbrains.com/help/clion/remote-projects-support.html)  
### 在Windows任务管理器中管理远程文件
Windows轻松使用：由于服务器没有安装FTP服务，所以无法在Windows资源管理器中直接添加网络位置，需借助第三方软件。  
**SFTP Net Drive** 或 Swish - Easy SFTP for Windows 或者 WinSCP，都基于SSH 的子协议SFTP。    
  下图为**SFTP Net Drive**的集成效果，可以支持更多本地化操作。  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/SFTP%20Net%20Drive.png)
### 在Windows中显示远程窗口
Windows下PUTTY+Xming实现远程程序窗口转发[教程](https://blog.csdn.net/u013554213/article/details/79885792)，可用于查看程显示的图片视频等。  
## 打印机配置
0.型号cannon imageCLASS lbp6230dw  
1.打印机设置为无线连接至402 professor wang, IP地址为10.21.6.97，在浏览器中访问该地址进行打印机管理，PIN密码为房间号。  
2.可以在自己电脑中添加网络打印机，IP如上，即可实现在本机发送打印任务。windwos添加后可以自动安装驱动，ubuntu、macOS需要在官网下载安装。  
3.打印机可以接受打印的IP地址限制为10.21.241.0-10.21.241.255（实验室）、10.21.6.66(单卡泰坦)、10.21.6.96（四卡泰坦）、10.21.240.96（隔壁实验室MIT无线路由器），如有需要，可以继续添加。  
4.办公室的两台电脑都已经可以正常打印。  
5.选择适合自己系统的版本下载驱动 [驱动网址](http://search-cn.canon-asia.com/canon__cn_zh__cn_zh/search.x?q=&ie=utf8&cat=0&ct=Support&pagemax=10&imgsize=1&pdf=ok&zoom=1&hf=category%09zubaken&cf=model_sm%3ALBP6230dw&modelName=LBP6230dw&ref=www.canon.com.cn&pid=Qwll_b5_PDEAbsajmlhBYg..&qid=X_aIwV7rUBVf7h04NiOK-9ugVEtFCiFs&d=DOWNLOADS%09Linux+64bit)

## 附加链接
[Linux分区：超过2TB硬盘分区](https://www.cnblogs.com/mannyzhoug/archive/2013/08/27/3284572.html)  
[Linux服务器新建用户和组，并分配sudo权限](https://www.cnblogs.com/devilmaycry812839668/p/10432877.html)  
