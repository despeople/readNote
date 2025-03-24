# 设置交叉编译工具链
```
CC = arm-rockchip830-linux-uclibcgnueabihf-g++
CFLAGS = -I/home/despeople/luckfox-pico/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/arm-rockchip830-linux-uclibcgnueabihf/sysroot/usr/local/include
LDFLAGS = -L/home/despeople/luckfox-pico/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/arm-rockchip830-linux-uclibcgnueabihf/sysroot/usr/local/lib
LIBS = -lopencv_core -lopencv_imgproc -lopencv_imgcodecs -ldl -lm -lpthread -lrt
```



# 目标文件和可执行文件
```
TARGET = resize_image
SRC = resize_image.cpp
OBJ = $(SRC:.cpp=.o)
```



# 默认目标
```
all: $(TARGET)
```



# 编译规则
```
$(TARGET): $(OBJ)
	$(CC) $(OBJ) -o $(TARGET) $(LDFLAGS) $(LIBS)
```



# 编译 .cpp 文件为 .o 文件
```
%.o: %.cpp
	$(CC) -c $< $(CFLAGS)
```

