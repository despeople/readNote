

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <sys/ioctl.h>
#include <linux/videodev2.h>
#include <sys/mman.h>

#define VIDEO_DEVICE "/dev/video11"
#define WIDTH  640
#define HEIGHT 480

int main() {
    int fd = open(VIDEO_DEVICE, O_RDWR);
    if (fd == -1) {
        perror("Failed to open video device");
        return errno;
    }

    // 设置V4L2格式
    struct v4l2_format format;
    memset(&format, 0, sizeof(format));
    format.type = V4L2_BUF_TYPE_VIDEO_CAPTURE_MPLANE;
    format.fmt.pix_mp.width = WIDTH;
    format.fmt.pix_mp.height = HEIGHT;
    format.fmt.pix_mp.pixelformat = V4L2_PIX_FMT_NV12;
    format.fmt.pix_mp.field = V4L2_FIELD_NONE;
    format.fmt.pix_mp.num_planes = 1; // 单平面
    format.fmt.pix_mp.plane_fmt[0].bytesperline = 640;             
    format.fmt.pix_mp.plane_fmt[0].sizeimage = 640 * 480 * 3 / 2;//yuv420  
    if (ioctl(fd, VIDIOC_S_FMT, &format) == -1) {
        perror("Failed to set video format");
        close(fd);
        return errno;
    }

    struct v4l2_requestbuffers reqbuf;
    memset(&reqbuf, 0, sizeof(reqbuf));
    reqbuf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE_MPLANE;
    reqbuf.memory = V4L2_MEMORY_MMAP;
    reqbuf.count = 1; // 仅需要一个缓冲区
    if (ioctl(fd, VIDIOC_REQBUFS, &reqbuf) == -1) {
        perror("Failed to request buffers");
        close(fd);
        return errno;
    }


    // 分配并映射缓冲区
    struct v4l2_buffer buf;
    struct v4l2_plane planes[VIDEO_MAX_PLANES];
    memset(&buf,  0, sizeof(buf));
    memset(planes,0, sizeof(planes));
    buf.type = V4L2_BUF_TYPE_VIDEO_CAPTURE_MPLANE;
    buf.memory = V4L2_MEMORY_MMAP;
    buf.index = 0;
    buf.length = VIDEO_MAX_PLANES;
    buf.m.planes = planes;
    buf.m.planes[0].bytesused = 0;
    if (ioctl(fd, VIDIOC_QUERYBUF, &buf) == -1) {
        perror("Failed to query buffer");
        close(fd);
        return errno;
    }


    void *buffer = mmap(NULL, buf.m.planes[0].length, PROT_READ | PROT_WRITE, MAP_SHARED, 
                        fd, buf.m.planes[0].m.mem_offset);
    if (buffer == MAP_FAILED) {
        perror("Failed to mmap buffer");
        close(fd);
        return errno;
    }

    // 将缓冲区放入视频流队列
    if (ioctl(fd, VIDIOC_QBUF, &buf) == -1) {
        perror("Failed to enqueue buffer");
        munmap(buffer, buf.length);
        close(fd);
        return errno;
    }

    // 开始视频流
    enum v4l2_buf_type type = V4L2_BUF_TYPE_VIDEO_CAPTURE_MPLANE;
    if (ioctl(fd, VIDIOC_STREAMON, &type) == -1) {
        perror("Failed to start streaming");
        munmap(buffer, buf.length);
        close(fd);
        return errno;
    }

    // 从视频流中捕获一帧图像
    if (ioctl(fd, VIDIOC_DQBUF, &buf) == -1) {
        perror("Failed to dequeue buffer");
        munmap(buffer, buf.length);
        close(fd);
        return errno;
    }

    // 将图像数据保存到文件中
    FILE *output_file = fopen("out.yuv", "wb");
    if (output_file == NULL) {
        perror("Failed to open output file");
        munmap(buffer, buf.length);
        close(fd);
        return errno;
    }
    fwrite(buffer, format.fmt.pix_mp.plane_fmt[0].sizeimage , 1, output_file);
    fclose(output_file);

    // 停止视频流
    if (ioctl(fd, VIDIOC_STREAMOFF, &type) == -1) {
        perror("Failed to stop streaming");
        munmap(buffer, buf.length);
        close(fd);
        return errno;
    }

    munmap(buffer, buf.length);
    close(fd);

    return 0;
}

```

