### 一、背景与目标  
以开源基准测试程序 coremark 为例，展示从 ruyi 包管理器安装到使用 ruyi 包管理器搭建 RISC-V 的编译、模拟环境，完成 coremark 源码本地编译并在 Licheepi 4A 开发板上运行的过程。  
### 二、环境说明  
- **硬件环境**：Licheepi 4A 开发板（th1520）  
- **软件环境**：Debian/openEuler for RISC-V  
### 三、ruyi 包管理器的安装  
参考 [RuyiSDK 包管理器下载安装](https://docs.ruyisdk.com/installation) 文档完成安装，本文档因硬件是 RISC-V 设备，需下载安装 riscv64 架构的 ruyi 版本。  
### 四、使用 ruyi 包管理器部署开发环境  
##### 1、查看软件仓软件包索引信息  
`$ ruyi list --name-contains gnu-plct --category-is toolchain`  
##### 2、安装 gnu 工具链  
`$ ruyi install gnu-plct-xthead`  
##### 3、查看预置编译环境  
`$ ruyi list profiles`  
##### 4、建立 ruyi 虚拟环境 `venv-sipeed`  
- 查看帮助：`$ ruyi venv -h`  
- 创建虚拟环境：  
`$ ruyi venv -t gnu-plct-xthead sipeed-lp14a venv-sipeed`  
- 查看编译环境中的工具：`$ ls venv-sipeed/bin/`  
- 激活虚拟环境：`$ . venv-sipeed/bin/ruyi-activate`  
- 验证 `gcc` 是否可用:  
`(venv-sipeed) $ riscv64-plctxthead-linux-gnu-gcc --version`  
### 五、准备编译对象（coremark 源码）  
##### 1、下载解压 coremark 源码  
创建并进入目录：  
`$ mkdir coremark && cd coremark`  
- 解压源码：`$ ruyi extract coremark`  
- 查看源码：`$ ls -al`  
### 六、交叉编译 coremark  
##### 1、设置 coremark 编译配置  
修改源码中的 `core_portme.mak` 文件，指定编译器：  
`$ sed -i 's/\/bgcc\//riscv64-plctxthead-linux-gnu-gcc\//g' linux64/core_portme.mak`  
##### 2. 执行交叉编译和构建  
生成可执行程序 `coremark.exe`：  
`$ make PORT_DIR=linux64 link $`  
`ls -al # 构建成功后可查看 coremark.exe`  
##### 3. 查看可执行程序属性  
`$ file coremark.exe # 查看文件架构等信息`  
### 六、运行验证  
直接运行 riscv64 架构的 `coremark` 可执行程序：  
`$ ./coremark.exe`  

