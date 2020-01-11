# manageQuadTitan
四卡泰坦电脑1的说明文档
## 警告警告警告
1. 由于服务器共享使用，不要随意重启服务器以免带来不必要的麻烦。  
2. 搞清楚每条命令的作用后再运行, 切忌盲目操作，尤其是需要sudo的命令。  

## 硬件概况

| 条目 | 详情 | 备注 |
| ------ | ------ | ------ |
| 启动项切换 | 开机按F8 |  |
| 处理器 | Intel E5 2678 V3 x2 |  |
| 主板 | ASUS Z10PE-D8 |  |
| 内存 | Samsung DDR4 RECC 2400Mhz 16GBx2 | 双通道，由于CPU内存控制器限制实际运行在2133Mhz |
| 硬盘 | Samsung 860 EVO 250GB SATA SSD + WD 2TB 5400rpm HDD  |  |
| 显卡 | Nvidia Titan Xp 12GB x4 |  |

## Ubuntu系统（机械硬盘）

### 环境
Ubuntu：16.04.4  
Nvidia Driver：384.130  
CUDA：9.0  
cuDNN：cuDNN v7.4.2 (Dec 14, 2018), for CUDA 9.0  
IP：10.21.241.96  
磁盘挂载：  
- 2TB机械为根目录 /
- 256GB固态中，32GB分区挂载为swap，剩余部分挂载在/mnt/256

### 公共账户
Username：root1root  
Password：112233

### 个人的账户和密码（通过SSH登录）
Username：姓名首字母缩写（小写）  
Password：姓名全拼（小写）  
请尽快修改密码  

| 姓名 | 用户名 | 密码 | 组 | 初始路径 | sudo权限  |
| ------ | ------ | ------ | ------ | ------ | ------ |
| *    璟 | hj | *** | teacher | /home/hj | 是 |
| 潘 * 昊 | pzh | *** | student | /home/pzh | 是 |
| *    腾 | wt | *** | student | /home/wt | 是 |
| 高 * 琦 | gmq | *** | student | /home/gmq | 是 |
| 罗 * 鹏 | llp | *** | student | /home/llp | 是 |
| 朱 * 丽 | zsl | *** | student | /home/zsl | 是 |
| 吴 * 鑫 | wsx | *** | student | /home/wsx | 是 |
| 刘 * 华 | llh | *** | student | /home/llh | 是 |
  
## 连接
### 在Windows中使用SSH连接工具
这里我使用PuTTY，它的自动登录设置可参考下图，快捷方式目标设定为形如`"C:\Program Files\PuTTY\putty.exe" -ssh 你的用户名@10.21.6.96 -pw 你的密码`，也可以通过创建密钥等其他方式进行自动登录。  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/PUTTY_autologin.png)  
### 在Windows任务管理器中管理远程文件
Windows轻松使用：由于服务器没有安装FTP服务，所以无法在Windows资源管理器中直接添加网络位置，需借助第三方软件。  
**SFTP Net Drive** 或 Swish - Easy SFTP for Windows 或者 WinSCP，都基于SSH 的子协议SFTP。    
  下图为**SFTP Net Drive**的集成效果，可以支持很多本地化操作。剪切大文件不需要网络传输，可瞬间完成，其他操作例如解压文件会先传输到本机，再传回服务器，所以应该直接用命令在服务器端解压和压缩。由于基础设施限制，理论传输网速为100Mb/s, 即12.5MB/s, 传输大文件的极限速度如上；若是大量零散的小文件（如解压后的图片数据集），整体速度会巨慢。  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/SFTP%20Net%20Drive.png)  
  下图为Advanced中的配置，字符集设置为UTF-8后可解决乱码问题，由于是linux系统，大小写敏感也需要勾选。连接后挂载的目录可以选择根目录，否则将无法返回根目录层级。 
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/sftp_charset.png)  
  ![图片无法加载](https://raw.githubusercontent.com/chwangteng/manageQuadTitan/master/sftp_case.png) 
- 值得注意的是，每个目录下用缩略图方式查看文件后会生成thumb.db，如果你的代码是**无脑读取目录下所有文件**的话，要记得手动删了这个东西哦。
### 在Windows中显示远程窗口
Windows下PuTTY+Xming实现远程程序窗口转发[教程](https://blog.csdn.net/u013554213/article/details/79885792)，可用于查看程显示的图片视频等。   
#### 远程图形界面常用指令
不指定程序名打开文件：`xdg-open test.jpg`  
#### 命令界面常用指令
- 查看目录内文件和文件夹大小：`du -sh ./*`  [参考](https://www.cnblogs.com/justforcon/archive/2017/12/02/7954481.html)  
- 统计目录下文件数量：`ls -l | grep "^-" | wc -l`  [参考](https://www.cnblogs.com/yongjieShi/p/8075281.html)  
- 统计目录下目录数量: `ls -l | grep "^d" | wc -l`  [参考](https://www.cnblogs.com/yongjieShi/p/8075281.html)  
- 删除含有大量文件的文件夹: `rsync --delete-before -d empty/ target/` [参考](https://blog.csdn.net/iamlihongwei/article/details/68060593) 
- 列出文件和目录的大小:`du -h --max-depth=1` [参考](https://blog.csdn.net/xiaoxinyu316/article/details/43269881)  
- 手动清理显存:当自己的程序已经中止，且`nvidia-smi`中已经看不到PID，但显存仍占用时，使用`sudo fuser -v /dev/nvidia*`查找自己程序占用GPU资源的PID，然后执行kill。  
- 查看进程运行的目录和命令：当`nvidia-smi`下显示的PID在`htop -p <PID>`中仍然显示不出详细命令和路径时，使用`cd /proc/<PID>`和 `ls -l exe`查看。  
- 创建软连接以将数据存储在空闲的磁盘仍保持目录的逻辑结构：`ln -s 原目录 映射目录`
## 运行计算
提示：Debug时使用CPU，就不会占用GPU资源。  
### 使用conda创建和管理环境
- 使用`conda`或 `pip`命令来创建和管理**你的环境**。使用`conda create -n yourenvname python=pythonversion`命令创建属于你的python环境，例如`conda create -n wtkeras python=2.7`。 创建的环境路径可通过`conda env list`命令查看。  
- 使用`conda env list`来查看目前存在和激活的的conda环境，使用`source activate yourenvname`来激活你的环境。安装GPU版的tf等框架建议使用pip命令，因为使用conda命令会自动下载对应的cuda和cudnn。关于conda、pip命令的更多使用方法，请参考其他教程。  
- **注意：本机当前安装的驱动程序及系统版本限制，不支持cuda9.2及以上版本，例如Pytorch最高只支持到1.1.0，详见各框架官网。**   
### 指定计算使用哪块GPU！指定计算使用哪块GPU！指定计算使用哪块GPU！   
- 运行程序前，务必使用如下命令来查看GPU的空闲情况, `-n`后指定刷新的时间间隔， `-d`高亮刷新部分。  
```linux
watch -n 1 -d nvidia-smi
```
- 例如在python中，在运行前，务必在程序中加入如下代码来指定抢占的GPU，x为0或1或2或3或0,1或0,2等（一共四块，为0123），同一块显卡可以运行多个程序，但是可能在某时刻一方程序会因为显存不足退出。如没有必要，不一次占用2块以上显卡。    
```python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = 'x' 
```  
也可以直接在终端的python命令前加上`CUDA_VISIBLE_DEVICES=0`来指定GPU。  
### 使用GPU加速
在/home/username/.bashrc中添加如下变量。
```linux
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64
export PATH=$PATH:/usr/local/cuda-9.0/bin
export CUDA_HOME=/usr/local/cuda-9.0
```
### 监控系统其他资源
- 使用[iostat工具](https://blog.csdn.net/feeltouch/article/details/95042396)`watch -n 1 -d iostat -d -x -k 1 2`  `watch -n 1 -d iostat 1 2`查看磁盘和CPU状况，使用`htop`  `top`查看CPU及进程，使用 `sudo iotop` 查看I/O状态，使用`bmon -p enp7s0`查看网络流量。
### 不间断运行python程序
- 使用形如`nohup python -u train.py &`命令保证进程不间断运行（如断开SSH连接等），`-u`可加可不加，它能保证python程序的输出可以无缓存、及时地更新到nohup.out文件中，对于不会主动停止的进程，训练后使用`kill`。  
- 使用`nohup python -u train.py > customfileout.txt 2>&1 &`形如这样的命令来指定输出文件名，2、1是标准输出流和错误流。  
更多用法参考[其他教程](https://blog.csdn.net/fang_chuan/article/details/82017470)  

### 使用Pycharm在远程环境中运行
- 如果使用Pycharm Professional进行远程开发、调试，可以参考[官方教程](https://www.jetbrains.com/help/pycharm/configuring-remote-interpreters-via-ssh.html)（配置时使用自己的ssh账号密码和自己创建的环境路径）并可以在Pycharm Professional的Remote Host中进行图形化文件管理。  
关于怎样免费使用Pycharm Professional，可在搜索引擎中搜索 **Jetbrain 学生** 或 **Github 学生包**   ，github学生认证更方便，通过后学生包中直接授权到Jetbrain的学生认证。   
  
- 如果提示`libcublas.so.9.0: cannot open shared object file: No such file`的话，将 `LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-9.0/lib64`添加到运行配置的pycharm环境变量中即可 。  

### 使用Clion在远程环境中运行
Clion远程配置[官方教程](https://www.jetbrains.com/help/clion/remote-projects-support.html)  
 
## 办公室打印机配置(没有钥匙)
- **型号**：cannon imageCLASS lbp6230dw  
- **后台**：打印机设置为有线连接, IP地址为10.21.6.97，在浏览器中访问该地址进行打印机管理，PIN密码为办公室房间号。    
- **IP限制**：打印机可以接受打印的IP地址限制为10.21.241.0-10.21.241.255（实验室）、10.21.6.66(单卡泰坦)、10.21.6.96（四卡泰坦）、10.21.240.96（隔壁实验室MIT无线路由器），如有需要，可以继续添加。办公室的两台电脑都可以打印。  
- **驱动程序**：可以在自己电脑中添加网络打印机，IP如上，即可实现在本机发送打印任务。windwos添加后可以自动安装驱动（Win10设置中，添加打印机和扫描仪，一段时间后出现并选择**我需要的打印机不在列表中**，选择**使用TCP/IP地址或主机名添加打印机**，主机名或IP地址中输入10.21.6.97,使用默认设置一路下一步即可。），ubuntu、macOS需要在官网下载安装。选择适合自己系统的版本下载驱动 [驱动网址](https://www.canon.com.cn/supports/download/sims/list/slist?fileTypeId=23&categoryId=15&seriesId=67&modelId=1146&OSName=Windows%2010%20(x64)&channel=1)
## 实验室打印机配置(耗材用完了)
- **型号**：同上  
- **后台**：网线连接，IP地址为10.21.240.57  
- **IP限制**：打印机可以接受打印的IP地址限制为10.21.241.0-10.21.241.255（417实验室）、10.21.240.0-10.21.240.255（423实验室），如有需要，可以继续添加。办公室的两台电脑都可以打印。 
- **驱动程序**：同上
## 附加链接
[Linux分区：超过2TB硬盘分区](https://www.cnblogs.com/mannyzhoug/archive/2013/08/27/3284572.html)  
