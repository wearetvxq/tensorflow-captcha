# 1x00   记录下安装等等坑
## 1x01 
在PRIME模式出来后 deepin 终于能成功安装gtx1050ti的显卡驱动了，加上我的U是改的八代
所以一直纠结了大半年，一直用虚拟机坚持下来的我终于可以完全享受Linux的快感了，所以测试了几次后
关键在于这一句，虽然之前也试过禁用闭源驱动等命令，终于在此成功了
acpi_osi=！acpi=”windows 2009“ 
sudo gedit /boot/grub/grub.cfg
这个文件需要加入
sudo gedit /etc/default/grub 
GRUB_CMDLINE_LINUX_DEFAULT="$GRUB_CMDLINE_LINUX_DEFAULT "`acpi_osi=! acpi_osi="Windows 2009`"
然后使用深度显卡管理的PRIME模式就能成功装上驱动了
# 2x00   关于cuda的安装（必须装9.0）
## 2x01
最新版本的9.2,直接浪费了一天的时间 即使成功装上了也用不了tensorflow，而且NVIDIA的demo是别想启动了
根本启动不了，试了很多中方法。在能够成功nvcc -V 和nvidia-smi后即可在Python中调试tensorflow了
根据报错来查询解决办法。其中主要的办法是找到文件 用软连接，因为版本的问题，显卡驱动是不能动的，所以更新驱动
的方法就无法实现了，只能找到同名称的文件 ln 软连接过去  其中文件有效目录为/usr/local/cuda-9.0/lib64

## 2x02
记录下版本和命令 
gcc版本  同时需要设置默认版本 也是通过软连接
不过即使安装的时候提示版本过高 7.3.3能 也可以通过-overlord 来完成安装
sudo apt-get install gcc-6 g++-6   
cuda9.0下载地址: https://developer.nvidia.com/cuda-90-download-archive
依次选择：linux -> x84_64 -> Ubuntu -> 16.04 -> runfile(local)
环境变量
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64/:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda-9.0/bin:$PATH

同时 /etc/ld.so.conf.d/加入文件 cuda.conf
内容为/usr/local/cuda-9.0/lib64 ，保存然后执行sudo ldconfig
能解决部分文件找不到问题   