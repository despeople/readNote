### 安装相关依赖工具

```
sudo apt update
sudo apt install -y git cmake
```

### 拉取RK官方MPP仓库

```
git clone https://github.com/rockchip-linux/mpp.git
```

进入aarch64相应的编译路径

```
cd mpp/build/linux/aarch64/
```

修改交叉编译配置文件，指定编译器gcc和g++（一般默认就好）

```
vim arm.linux.cross.cmake
```

arm.linux.cross.cmake

```cmake
SET(CMAKE_SYSTEM_NAME Linux)
SET(CMAKE_C_COMPILER "aarch64-none-linux-gnu-gcc")
SET(CMAKE_CXX_COMPILER "aarch64-none-linux-gnu-g++")
SET(CMAKE_SYSTEM_PROCESSOR "armv8-a")

# 安装路径（FORCE确保覆盖缓存）
set(CMAKE_INSTALL_PREFIX "/home/rk3588j/rkmpp/install" CACHE PATH "rkmpp install dir" FORCE)

add_definitions(-fPIC)
add_definitions(-DARMLINUX)
add_definitions(-Dlinux)
```

运行bash脚本后编译

```
./make-Makefiles.bash
make
```

