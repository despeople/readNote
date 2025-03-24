# 单独编译OPENCV



```shell
tar zxvf opencv-xxx.tar.gz

cd opencv-xxx

mkdir build 

cd build

touch toolchain.cmake

nano toolchain.cmake
```

写入

```cmake
set( CMAKE_SYSTEM_NAME Linux )
 
set( CMAKE_SYSTEM_PROCESSOR arm (arm64) )

set( CMAKE_C_COMPILER 交叉编译链名-gcc )

set( CMAKE_CXX_COMPILER 交叉编译链名-g++ )

set(CMAKE_INSTALL_PREFIX "安装路径（绝对路径）" CACHE PATH "Description of the install prefix" FORCE)
```

保存后关闭，命令行输入：

```shell
cmake -DCMAKE_TOOLCHAIN_FILE=toolchain.cmake ../

make -j2

make install 
```

