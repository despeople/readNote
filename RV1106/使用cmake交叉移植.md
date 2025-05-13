使用 CMake 进行交叉编译并移植代码到开发板，并且链接动态库的过程分为几个步骤。涵盖了交叉编译工具链的设置、CMake 配置、以及如何链接动态库。

### **1. 安装交叉编译工具链**

首先，确保你在主机上安装了适用于目标开发板的交叉编译工具链。例如，如果你要将代码移植到 ARM 开发板上，可能会使用 `arm-linux-gnueabihf-gcc` 作为交叉编译器。

可以通过包管理器安装工具链，或从开发板供应商获取交叉编译工具链。

在 Ubuntu 上安装 ARM 工具链的示例：

```bash
sudo apt-get install gcc-arm-linux-gnueabihf
```

### **2. 创建交叉编译工具链文件**（这个文件我用的是移植opencv时build目录下的toolchain.cmake其实就是配置都大差不差只要在后面配置步骤指定路径就可以，也可以在自己的项目下的build目录下自己再写一个）

你需要为 CMake 创建一个工具链文件来指定交叉编译的环境，包括目标平台的编译器、链接器、库路径等信息。下面是一个简单的交叉编译工具链文件 `toolchain.cmake` 的示例：

```cmake
# toolchain.cmake

# 指定目标系统的架构
SET(CMAKE_SYSTEM_NAME Linux)
SET(CMAKE_SYSTEM_PROCESSOR arm)

# 设置交叉编译工具链路径
SET(CMAKE_C_COMPILER /home/despeople/luckfox-pico/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf-gcc)
SET(CMAKE_CXX_COMPILER /home/despeople/luckfox-pico/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf-g++)

# 设置目标系统的根文件系统
SET(CMAKE_FIND_ROOT_PATH /home/despeople/luckfox-pico/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/arm-rockchip830-linux-uclibcgnueabihf/sysroot)

# 告诉 CMake 只使用交叉编译工具链搜索库和头文件
SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
```

在 `toolchain-arm.cmake` 文件中，你可以根据目标开发板的具体环境进行调整，例如工具链路径和目标系统的架构（`arm`、`aarch64` 等）。

### **3. 编写 CMakeLists.txt 文件**

在项目根目录中，确保你的 `CMakeLists.txt` 文件已经准备好，可以处理交叉编译，并且正确地链接动态库。以下是一个简单的 CMake 配置示例：

```cmake
cmake_minimum_required(VERSION 3.22.1)  # 选择 CMake 版本
project(changecolor)

find_package(OpenCV REQUIRED)  # 查找 OpenCV
include_directories(${OpenCV_INCLUDE_DIRS}/home/despeople/luckfox-pico/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/arm-rockchip830-linux-uclibcgnueabihf/sysroot/usr/local/include)  # 添加 OpenCV 头文件路径

add_executable(main changeColor.cpp)  # 指定编译 changeColor.cpp 自己需要编译的文件名 前面的main就是自己想要的可执行文件的名字可以修改为自己想要的，如果修改了那么下面的main也要修改为相同的名字

target_link_libraries(main ${OpenCV_LIBS})  # 链接 OpenCV 库 

```

如果你的项目依赖于其他动态库（例如你开发的特定库），你需要确保它们的路径被正确设置。

```cmake
# 如果你需要链接其他库，可以使用以下语法
target_link_libraries(my_program /path/to/your/library.so)
```

### **4. 配置交叉编译**

使用 CMake 配置命令时，指定交叉编译工具链文件来确保交叉编译。你可以在终端运行如下命令来构建项目：

指定toolchain.cmake路径

```bash
mkdir build
cd build
cmake .. -DCMAKE_TOOLCHAIN_FILE=../toolchain.cmake
make
```

### **5. 传输到开发板**

交叉编译完成后，你可以将生成的二进制文件和依赖的动态库传输到目标开发板上。

常用的传输方式包括：

- **通过 SSH 和 SCP**：
  使用 `scp` 命令将文件传输到开发板。例如：

  ```bash
  # 传输文件
  scp ssh-test.txt root@172.32.0.93:/root
  # 传输文件夹
scp -r ssh-test root@172.32.0.93:/root
  ```
  
- **使用 USB 或 SD 卡**：将编译结果通过 USB 或 SD 卡复制到开发板。

### **6. 在开发板上运行**

进入开发板的终端，切换到存放程序的位置，然后运行程序。如果程序依赖于动态库，确保将动态库放在开发板的某个目录中（比如 `/lib` 或 `/usr/lib`）。

你可以通过 `LD_LIBRARY_PATH` 环境变量指定动态库的查找路径，例如：

```bash
export LD_LIBRARY_PATH=/home/user/libs:$LD_LIBRARY_PATH
./my_program
```

### **7. 调试**

如果程序运行出现问题，可以通过在开发板上运行 `gdb` 调试程序，或者检查日志输出帮助排查问题。

---

### **附加说明**

- **库路径设置**：如果你编译了动态库，并且想要在 CMake 中链接这些库，记得在 `CMakeLists.txt` 中正确指定库的路径。例如，使用 `link_directories` 和 `target_link_libraries`：
  ```cmake
  link_directories(/path/to/your/libs)
  target_link_libraries(my_program library_name)
  ```

- **目标开发板的文件系统**：如果开发板使用的是嵌入式 Linux 系统，通常可以将库和二进制文件拷贝到开发板的文件系统路径下（如 `/usr/local/lib`）以供运行时加载。

- **调试和日志输出**：如果目标设备没有调试器支持，你可能需要通过添加日志输出（例如使用 `printf` 或日志库）来调试代码。

---

### **总结**

使用 CMake 进行交叉编译的关键是设置正确的工具链文件，并且在 CMake 中正确配置目标平台和依赖的动态库路径。通过这些步骤，你可以在本地开发环境中编译适用于开发板的代码，并顺利移植到目标设备上运行。

![image-20250430185820517](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250430185820517.png)