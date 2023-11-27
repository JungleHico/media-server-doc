# 基于 lalserver 和 FFmpeg 搭建本地流媒体服务测试环境

## 安装 lalserver

从 [lalserver仓库](https://github.com/q191201771/lal/releases) 下载对应版本的已编译好的二进制可执行文件，然后解压缩

## 安装 FFmpeg

参考 [FFmpeg 官网](https://ffmpeg.org/download.html)，安装对应系统版本，例如 Ubuntu：

```sh
sudo apt install ffmpeg
```

可能需要额外安装某些编解码器，根据提示执行即可

## 启动 lalserver

```sh
./bin/lalserver -c conf/lalserver.conf.json
```

## FFmpeg 推流

```sh
ffmpeg -re -stream_loop -1 -i ./video.mp4 -c copy -f rtsp rtsp://localhost:5544/live/test
```

- `-re` 表示根据视频文件的帧率模拟实时视频流
- `-stream_loop -1` 表示循环播放
- `-i` 用于指定输入文件路径
- `-c copy` 表示复制视频而不进行重新编码
- `-f rtsp rtsp://localhost:5544/live/test` 指定输出格式为 RTSP 流，并指定输出流地址

其他协议的推流地址参考：[lalserver 各协议推拉流url地址列表](https://pengrl.com/lal/#/streamurllist)

## 播放 RTSP 流

```sh
ffplay rtsp://localhost:5544/live/test
```