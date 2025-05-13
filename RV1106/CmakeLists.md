```cmake
cmake_minimum_required(VERSION 3.4.1)
project(PROJECT_SOURCE_DIR)

# 设置可执行文件名
set(EXECUTABLE_FILE_NAME "test_file")

# 链接阶段可以容忍某些函数或变量没有在当前链接的目标文件或者库中定义，它们可以在运行时从其他加载的动态库中获取
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wl,--allow-shlib-undefined")
# 链接阶段可以容忍某些函数或变量没有在当前链接的目标文件或者库中定义，它们可以在运行时从其他加载的动态库中获取
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wl,--allow-shlib-undefined")
# 链接阶段可以容忍某些函数或变量没有在当前链接的目标文件或者库中定义，它们可以在运行时从其他加载的动态库中获取
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,--allow-shlib-undefined")

# 常用于确保安装后的应用程序能够在其安装目录下找到相关的依赖库，无需额外配置系统的全局库路径
set(CMAKE_SKIP_INSTALL_RPATH FALSE)
# 让构建阶段生成的可执行文件或库具有与将来安装后相同的库查找机制
set(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
# 指定安装后的运行时库查找路径为安装目录下的lib子目录。这意味着当程序运行时，会优先在这个目录下查找依赖的动态库文件
set(CMAKE_INSTALL_RPATH "$ORIGIN/lib")
set(CMAKE_BUILD_TYPE Debug)
add_compile_options(-g)


# 设置一些库的路径
# set(XXX_LIB_PATH ${PROJECT_SOURCE_DIR}/xxxlib)
# set(XXX_LIB 
# ${XXX_LIB_PATH}/xxxlib1.so
# ${XXX_LIB_PATH}/xxxlib2.so
#)
# set(XXX_LIB_PATH ${PROJECT_SOURCE_DIR}/xxxlib)
# set(XXX_LIB 
# ${XXX_LIB_PATH}/xxxlib1.so
# ${XXX_LIB_PATH}/xxxlib2.so
#)


# 扫描文件夹下的所有源文件
aux_source_directory(${PROJECT_SOURCE_DIR}/src SOURCE)

# 添加可执行文件
add_executable(${EXECUTABLE_FILE_NAME}
${SOURCE}    
)


# 链接头文件  
target_include_directories(${EXECUTABLE_FILE_NAME} PUBLIC
${PROJECT_SOURCE_DIR}/include
)

# 链接库
target_link_libraries(${EXECUTABLE_FILE_NAME} PUBLIC
#${XXX_LIB}
)


# 设置安装路径
set(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR}/install/bin)

# 安装可执行文件 
install(TARGETS ${EXECUTABLE_FILE_NAME} DESTINATION ./)

# 安装库
#install(PROGRAMS ${XXX_LIB} DESTINATION lib)

# 安装文件
#install(FILES run.sh DESTINATION ./)

# 安装目录
#install(DIRECTORY model img cache DESTINATION ./)

```

