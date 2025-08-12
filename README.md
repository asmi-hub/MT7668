## S905L Armbian 内核 6.1.x 适配编译指南

##### 创建工具链目录并下载
```bash
mkdir -p /usr/local/toolchain  
cd /usr/local/toolchain  
wget https://github.com/ophub/kernel/releases/download/dev/arm-gnu-toolchain-13.3.rel1-aarch64-aarch64-none-elf.tar.xz  
tar -Jxf arm-gnu-toolchain-13.3.rel1-aarch64-aarch64-none-elf.tar.xz
```
##### 配置环境变量
```bash
echo 'export PATH=$PATH:/usr/local/toolchain/arm-gnu-toolchain-13.3.rel1-aarch64-aarch64-none-elf/bin/' | tee -a /etc/profile.d/gcc-aarch64-none-elf.sh  
source /etc/profile
```
##### 创建符号链接
```bash
ln -sf /usr/local/toolchain/arm-gnu-toolchain-13.3.rel1-aarch64-aarch64-none-elf/bin/aarch64-none-elf-gcc /usr/local/bin/gcc
```
##### 克隆驱动仓库
```bash
cd /root  
git clone https://github.com/asmi-hub/MT7668.git  
cd MT7668
```
##### 使用指定 Makefile 编译
```bash
make EXTRA_CFLAGS="-w" CROSS_COMPILE= -f Makefile -j$(nproc)  
```
##### 复制固件和驱动模块
```bash
cp ~/MT7668/7668_firmware/* /usr/lib/firmware/  
cp -f ~/MT7668/drv_wlan/MT7663/wlan/wlan_mt76x8_sdio.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless/
```
##### 更新模块依赖并加载
```bash
depmod -a  
modprobe wlan_mt76x8_sdio
```
##### 验证模块加载
```bash
lsmod
```
