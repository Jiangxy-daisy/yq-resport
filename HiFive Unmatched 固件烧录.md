### 操作系统信息  
- 系统版本：Debian trixie/sid  
### 硬件信息  
- HiFive Unmatched Rev A  
- microUSB 线缆一条（随 HiFive Unmatched 附赠）  
- ATX 电源一个  
- microSD 卡一张  
- M.2 NVMe SSD 一个  
- M.2 转 USB 硬盘盒，用于镜像刷写  
## 安装步骤  
根据 [软件参考手册](https://www.sifive.cn/document-file/hifive-unmatched-software-reference-manual)，HiFive Unmatched 有多种启动方式。  
- 通过Windows下载镜像文件  
- 首先将sd卡接入电脑，下载`https://github.com/yuzibo/Unmatched-Debian-image/releases/download/0.0.5-beta/sd-uboot.img.xz`   
- 使用Rufus工具进行烧录  
- 接着将nvme ssd固态硬盘接入电脑，下载`https://github.com/yuzibo/Unmatched-Debian-image/releases/download/0.0.5-beta/nvme-rootfs.img.xz`   
- 使用Rufus工具进行烧录  
### 登录系统  
- 通过板载串口（使用 microUSB 线缆连接至其他计算机）登录系统。  
- 默认用户名：`rv` 默认密码：`rv`  
### 启动信息  
```
Debian GNU/Linux trixie/sid unmatched ttySIF0

unmatched login: rv
Password:
Linux unmatched 6.12.17-riscv64 #1 SMP Debian 6.12.17-1 (2025-03-01) riscv64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
rv@unmatched:~$ fastfetch
        _,met$$$$$gg.          rv@unmatched
     ,g$$$$$$$$$$$$$$$P.       ------------
   ,g$$P""       """Y$$.".     OS: Debian GNU/Linux trixie/sid riscv64
  ,$$P'              `$$$.     Host: SiFive HiFive Unmatched A00
',$$P       ,ggs.     `$$b:    Kernel: Linux 6.12.17-riscv64
`d$$'     ,$P"'   .    $$$     Uptime: 50 seconds
 $$P      d$'     ,    $$P     Packages: 186 (dpkg)
 $$:      $$.   -    ,d$$'     Shell: bash 5.2.37
 $$;      Y$b._   _,d$P'       Terminal: vt220
 Y$$.    `.`"Y$$$$P"'          CPU: fu740-c000 (4)
 `$$b      "-.__               Memory: 294.54 MiB / 15.60 GiB (2%)
  `Y$$b                        Swap: Disabled
   `Y$$.                       Disk (/): 545.80 MiB / 2.49 GiB (21%) - ext4
     `$$b.                     Local IP (end0): 10.0.0.117/24
       `Y$$b.                  Locale: C
         `"Y$b._
             `""""

rv@unmatched:~$ uname -a
Linux unmatched 6.12.17-riscv64 #1 SMP Debian 6.12.17-1 (2025-03-01) riscv64 GNU/Linux
rv@unmatched:~$ cat /proc/cpuinfo
processor       : 0
hart            : 2
isa             : rv64imafdc_zicntr_zicsr_zifencei_zihpm_zca_zcd
mmu             : sv39
uarch           : sifive,bullet0
mvendorid       : 0x489
marchid         : 0x8000000000000007
mimpid          : 0x20181004
hart isa        : rv64imafdc_zicntr_zicsr_zifencei_zihpm_zca_zcd

processor       : 1
hart            : 1
isa             : rv64imafdc_zicntr_zicsr_zifencei_zihpm_zca_zcd
mmu             : sv39
uarch           : sifive,bullet0
mvendorid       : 0x489
marchid         : 0x8000000000000007
mimpid          : 0x20181004
hart isa        : rv64imafdc_zicntr_zicsr_zifencei_zihpm_zca_zcd

processor       : 2
hart            : 3
isa             : rv64imafdc_zicntr_zicsr_zifencei_zihpm_zca_zcd
mmu             : sv39
uarch           : sifive,bullet0
mvendorid       : 0x489
marchid         : 0x8000000000000007
mimpid          : 0x20181004
hart isa        : rv64imafdc_zicntr_zicsr_zifencei_zihpm_zca_zcd

processor       : 3
hart            : 4
isa             : rv64imafdc_zicntr_zicsr_zifencei_zihpm_zca_zcd
mmu             : sv39
uarch           : sifive,bullet0
mvendorid       : 0x489
marchid         : 0x8000000000000007
mimpid          : 0x20181004
hart isa        : rv64imafdc_zicntr_zicsr_zifencei_zihpm_zca_zcd

rv@unmatched:~$ cat /etc/os-release
PRETTY_NAME="Debian GNU/Linux trixie/sid"
NAME="Debian GNU/Linux"
VERSION_CODENAME=trixie
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
rv@unmatched:~$ cat /etc/deb
debconf.conf    debian_version
rv@unmatched:~$ cat /etc/debian_version
trixie/sid
```  
这里就烧录并且登陆成功了