将gt911驱动放在下面目录下

```
/home/despeople/luckfox-pico/sysdrv/source/kernel/drivers/input/touchscreen/gt9xx
```

但是系统没办法直接识别/gt9xx/Kconfig配置文件，这时候就需要修改以下几个地方：

### **1. 修改父级 Makefile**

编辑顶层触摸屏驱动 `Makefile`：

```bash
nano drivers/input/touchscreen/Makefile
```

在文件 **末尾** 添加（确保无重复）：

```makefile
obj-$(CONFIG_TOUCHSCREEN_GT9XX) += gt9xx/
```

### **2. 检查子目录 Makefile**

确认 `gt9xx/Makefile` 内容应为：

```bash
cat drivers/input/touchscreen/gt9xx/Makefile
```

预期输出类似：

```makefile
obj-$(CONFIG_TOUCHSCREEN_GT9XX) += gt9xx.o gt9xx_update.o goodix_tool.o
```

### **3. 链接 Kconfig**

在父级 `Kconfig` 中添加引用：

```bash
nano drivers/input/touchscreen/Kconfig
```

在文件 **末尾** 添加：

```kconfig
source "drivers/input/touchscreen/gt9xx/Kconfig"
```

### **4. 强制重建配置**

```bash
# 清除旧配置
make ARCH=arm distclean
# 重新加载默认配置（Luckfox Pico 通常使用 rockchip_linux_defconfig）
make ARCH=arm rockchip_linux_defconfig
# 检查 GT9XX 是否出现
make ARCH=arm menuconfig
```

这时就会出现

```
-> Device Drivers
  -> Input device support
    -> Touchscreens
      [*] Goodix touchpanel GT9xx series 
```

