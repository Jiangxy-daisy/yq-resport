今天是刷写LicheePiA镜像的详细过程，我选择的是用Windows系统烧录

首先刷写需要的相关环境如下：  
①系统及版本：Windows 11 24H2 OS内部版本26100.3194  
②架构： x86_64  
③LicheePi4A 板卡规格为16G RAM + 128G eMMC  

关于刷写镜像的目的，我进行了总结：类似于给开发板安装操作系统，但还包含了适配硬件的引导程序等特殊环节，是让开发板能正常运行的完整系统部署过程。

这里有两种启动镜像的方式：  
1.从SD card启动  
2.从eMMC启动  
与此同时，刷写镜像的途径分为连接串口进行刷写与不连接串口进行刷写两种情况，考虑到接入串口进行刷写不是必须行为，所以默认是使用不连接串口的刷写方式。我这里选择的是从eMMC启动且不接入串口（要取出SD card）

**第一步：安装镜像刷写工具fastboot**
①访问 Android 开发者官网  
[https://developer.android.com/studio/releases/platform-tools](https://developer.android.com/studio/releases/platform-tools)   
找到适合 Windows 系统的 Android SDK Platform-Tools 压缩包进行下载。  
②安装完成后，在包含`fastboot.exe`的文件夹中打开Powershell，运行`.\fastboot.exe --version`，判断`fastboot.exe`是否能够正常运行。

**第二步：获取镜像**  
稳定版本（5.10内核）：  
https://mirror.iscas.ac.cn/revyos/extra/images/lpi4a/20240720/  
最新版本（6.6内核）：  
https://mirror.iscas.ac.cn/revyos/extra/images/lpi4a/20250526/  
//注意：根据开发板的内存版本选择合适的uboot文件（扫描开发板上的二维码，第二部分的数字便是核心板的内存+存储配置）  
①确认好板卡规格后，下载、校验并解压对应的 uboot、boot以及root文件（uboot文件不需要解压）  
②最好将这三个文件放在fastboot同级目录下

**第三步：连接板卡并打入驱动**  
//注意：进行烧录的过程中不要用电源适配器给开发板供电，而是通过type-c口的数据线  
①按住板卡上的BOOT键后，将靠近BOOT键的Type-C口接入电脑，板卡会进入刷写模式。  
②win+X，打开设备管理器，如果在“其他设备”处看到“USB download gadget”，即表明设备已被正确识别。但是未安装驱动程序。  
③为了打入fastboot驱动，需要下载Google USB驱动程序下载并解压到某一位置  
https://developer.android.com/studio/run/win-usb?hl=zh-cn  
④右键设备管理器中的“USB download gadget”，点击“更新驱动程序”  
⑤选择“浏览我的电脑以查找驱动程序”  
⑥选择“让我从计算机上的可用驱动程序列表中选取”  
⑦选中“显示所有设备”，并点击“下一步”  
⑧点击“从磁盘安装”  
⑨点击“浏览”，选中Google USB驱动下的inf文件，点击确定  
⑩选中“Android Bootloader Interface”，点击“下一步”，在弹出对话框中点击“是”，在弹出的Windows安全中心对话框中点击“安装”  
至此就安装成功了  

**第四步：刷写镜像**  
①回到包含`fastboot.exe`的Powershell终端，输入下面的命令，程序应当输出一行信息代表有一个设备已经连接。  
 .\fastboot.exe devices  
②输入下面的命令以刷写并重启到临时uboot：  
.\fastboot.exe flash uboot .\u-boot-with-spl-lpi4a-16g.bin   
.\fastboot reboot  
③开发板重启后，计算机可能再次检测到一个名为`USB download gadget`的未知设备。请按照上面的驱动安装教程重新为该设备安装驱动后继续下面的步骤。  
.\fastboot.exe flash uboot 文件绝对路径  
.\fastboot.exe flash boot 文件绝对路径  
.\fastboot.exe flash root 文件绝对路径  
//注意：如果遇到安装驱动失败，就将板卡的usb线拔掉，重新按住BOOT键接入，再重新刷写一下镜像就可以了  
④至此镜像刷写完成，可以通过重新拔插电源线的方式启动系统。  

重新启动的过程我遇到过无法开机，显示器出现四只企鹅无法跳转，串口调试工具出现initramfs的情况，说明启动失败了，镜像可能烧录的过程出现一点差错，检查一下内存文档是否下载错了，再重新刷写一下镜像就解决了。