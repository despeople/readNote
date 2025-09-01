## 检查设备是否识别：

```bash
adb devices
```

## **推送文件到设备 (PC → 设备)**

```bash
adb push <本地文件路径> <设备目标路径>
adb push D:\postgraduates\embedded\嵌入式+AI\RK3588J\yolov5_video_stream_multithread\install /home/firefly/Desktop/firefly/
```

#### **从设备拉取文件 (设备 → PC)**

```bash
adb pull <设备文件路径> <本地目标路径>
```

#### **传输整个目录**

添加 `-a` 保留文件属性（如时间戳）：

```bash
adb push -a ./local_dir/ /device_dir/
adb pull -a /device_dir/ ./local_dir/
```