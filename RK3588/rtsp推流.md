获取ZLMediaKit：https://github.com/ZLMediaKit/ZLMediaKit/wiki/%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B直接放板子上编译

查看服务器和推流地址能否ping通

将main.cpp的URL修改为要推向的服务器的地址

![image-20250821152427253](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250821152427253.png)

ffplay的地址就是拉取服务器的地址

```
ffplay -fflags nobuffer -flags low_delay -framedrop -strict experimental -analyzeduration 1000000 -probesize 1000000 rtsp://10.5.118.38:8554/stream
```

