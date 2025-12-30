## 一、核心目标  
在 QEMU 模拟器 + LLVM 工具链环境中，为 MilkV Duo 编译并运行 Coremark  
## 二、所需工具  
- 包管理工具：`ruyi`（MilkV 官方工具）  
- 工具链：LLVM、GNU-plct（RISC-V 编译环境）  
- 模拟器：QEMU-user-riscv（RISC-V 架构模拟）  
## 三、步骤详解  

### 1. 安装依赖  
打开终端，执行：  
`ruyi install llvm-upstream gnu-plct qemu-user-riscv-upstream`  
### 2. 创建 + 激活虚拟环境  
- 创建环境：  
`ruyi venv -t llvm-upstream --sysroot-from gnu-plct -e qemu-user-riscv-upstream generic ~/venv`  
- 激活环境：  
`. ~/venv/bin/ruyi-activate`  
### 3. 解包 Coremark 并配置编译规则  
- 准备目录：  
`mkdir coremark && cd coremark`  
- 解包源码：  
`ruyi extract coremark`  
- 进入源码目录：  
`cd coremark-1.0.1`  
- 修改编译器配置：  
`sed -i 's/CC = .*/CC = riscv64-unknown-linux-gnu-gcc/' linux64/core_portme.mak`  
### 4. 编译 Coremark  
执行编译命令：`make PORT_DIR=linux64`  
### 5. QEMU 运行 Coremark  
测试性能：`ruyi-qemu coremark.exe`  