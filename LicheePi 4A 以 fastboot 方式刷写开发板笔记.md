### 一、环境准备  
- 硬件：x86_64 架构 PC  
- 软件：Ubuntu 22.04 系统  
### 二、设备连接与 fastboot 配置  
- 连接设备：将 LicheePi 4A 用 USB 线接入上位机 PC，选择 fastboot 方式刷写  
- 配置 udev 规则（解决设备权限问题）：  
`sudo nano /etc/udev/rules.d/51-android.rules`  
- 在文件中添加以下内容（LicheePi 4A 对应的 Vendor/Product ID）：  
`SUBSYSTEM=="usb", ATTR{idVendor}=="2345", ATTR{idProduct}=="7654", MODE="0666", GROUP="plugdev"  
`SUBSYSTEM=="usb", ATTR{idVendor}=="1234", ATTR{idProduct}=="8888", MODE="0666", GROUP="plugdev"  
- 保存退出（`Ctrl+O`→回车→`Ctrl+X`），重载规则：  
`sudo udevadm control --reload-rules && sudo udevadm trigger`   
- 将当前用户加入 plugdev 组（需重新登录生效）：  
`sudo usermod -aG plugdev $USER`  
- 打开新终端，执行:  
`fastboot devices`   
若输出类似 `0123456789abcdef fastboot` 的内容，说明配置成功  
### 三、刷写系统（通过 ruyi 包管理器）  
1. **确认 ruyi 已安装**：  
- 终端执行 `ruyi -v`，验证是否能输出版本信息  
1. **启动刷写向导**  
2. 运行：`ruyi device_provision`
3. **按向导选择配置**：  
    - 输入 `y` 确认继续  
    - 选择设备：输入 `7`（对应 StarFive LicheePi 4A）  
    - 选择硬件版本：输入 `2`（对应 Sipeed LicheePi 4A (16G RAM)）  
    - 选择系统：输入对应序号（如 `3` 选择 Reyvos 系统）  
    - 输入 `y` 确认开始下载并刷写  
### 四、注意事项  
- 若 `fastboot devices` 无输出，需检查 USB 连接、设备是否进入 fastboot 模式  
- 刷写过程中不要断开设备连接，等待向导执行完成  
- 这里我使用的是VMware虚拟机新建了Ubuntu22.04系统进行的操作，虚拟机上要提前安装好fastboot和ruyi包管理器

`
