## 一、编译环境搭建（基于 Ruyi 工具）  
1. 安装 MilkV Duo 编译工具链  
`ruyi install gnu-milkv-milkv-duo-musl-bin`  
2. 创建虚拟环境  
`ruyi venv -t gnu-milkv-milkv-duo-musl-bin generic milkv-venv`  
3. 激活虚拟环境  
`. milkv-venv/bin/ruyi-activate`  
激活后终端前缀会显示 `«Ruyi milkv-venv»`  
## 二、获取 Coremark 源码  
1. 创建并进入源码目录  
`mkdir coremark && cd coremark`  
2. 从 Ruyi 仓库下载并解压源码  
`ruyi extract coremark`  
（会自动下载 `coremark-1.01.tar.gz` 并解压到当前目录）  
## 三、修改构建脚本  
因工具链为 `gnu-milkv-milkv-duo`，需替换编译脚本中的编译器路径：  
`sed -i 's/\/bgcc\/b/riscv64-unknown-linux-musl-gcc/g' linux64/core_portme.mak`  
## 四、编译 Coremark  
执行构建命令（指定架构为 RISC-V 64）：  
`make PORT_DIR=linux64 LFLAGS_END=-march=rv64gcv0p7xthead link`  
验证编译结果（确认是 RISC-V 架构二进制）：  
`file coremark.exe`  
输出示例：`coremark.exe: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI`  
## 五、退出虚拟环境  
`ruyi-deactivate`  
## 六、传到 MilkV Duo 并运行  
1. 传输二进制文件（替换 IP 为设备实际地址）  
`scp -O ./coremark.exe root@192.168.42.1:~`  
2. 在 MilkV Duo 上运行  
`./coremark.exe`  
运行结果示例：  
```plaintext
2K performance run parameters for coremark.
CoreMark Size   : 666
Total ticks     : 14911
Total time (secs): 14.911000
Iterations/Sec  : 2011.937496
...
CoreMark 1.0 : 2011.937496 / GCC13.1.0 -O2 -static / Heap
```  

