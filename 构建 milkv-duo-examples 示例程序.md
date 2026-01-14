## 一、前期准备：安装依赖工具链  
打开终端，执行：`ruyi install gnu-milkv-milkv-duo-musl-bin`  
## 二、创建目录并解压示例包  
1.新建工作目录：  
`mkdir test-duo-examples`  
2.进入目录：  
`cd test-duo-examples`  
3.解压示例程序：  
`ruyi extract milkv-duo-examples`  
## 三、环境配置  
- 无需手动修改脚本内容，直接在当前目录执行：  
`source envsetup.sh`  
- 执行后选择对应设备（输入 1/2），看到`Environment is ready`即配置完成。  
## 四、编译示例程序  
- 以`i2c/sd1306_i2c`示例为例：  
1.进入示例子目录：  
`cd i2c/sd1306_i2c`  
2.执行编译：  
`make`  

