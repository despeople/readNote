# lvgl静态库编译 基于v8.3

v8.3版本为LVGL跨入9之前最后一个版本，之后9版本不再兼任8.3

从https://github.com/pan1024/lv_port_linux_frame_buffer-8.3获取lv_port_linux_frame_buffer 

文件夹内有CMakeList.txt，lv_conf.h，lv_drv_conf.h，需要修改：

## CMakeList.txt

为了保险起见请在project(xxx)前添加

set(CMAKE_C_COMPILER "交叉编译链完整路径")#指定编译链完整路径
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99")
set(CMAKE_INSTALL_PREFIX "安装路径") #安装路径

示例：

set(CMAKE_C_COMPILER "/home/pan/arm-gnu-toolchain-11.3.rel1-x86_64-arm-none-linux-gnueabihf/bin/arm-none-linux-gnueabihf-gcc")#指定编译链完整路径
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99")
set(CMAKE_INSTALL_PREFIX "/home/pan/lvgl_result") #安装路径

## lv_conf.h，lv_drv_conf.h：

这两个文件内的配置将影响后面生产的静态（动态）库功能，其内部所激活的选项才会被编译到库中，因此在一个项目可用的库，在另一个项目中不一定可用

## 编译

配置完成以上内容后执行在当前文件夹下执行cmake ./

配置完成以上内容后即可make -jx 其中x为cpu核心数如make -j2

编译完成后使用make install将库（lib）和头文件（include）安装到上方设置的安装路径

这里注意其产生的include为了更加简洁需要一定修改

include内有lvgl文件夹，lvgl内则有lv_conf.h和lv_drivers文件夹，将lv_conf.h删除，同时将lv_drivers文件夹内的全部内容移到lvgl文件夹(或者include)下，以此优化文件夹路径