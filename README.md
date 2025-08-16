# nginx-rtmp-httpflv
🚀 自动构建 Windows 版 nginx

### ✅ 功能
- RTMP 推流 / 播放
- HTTP-FLV 播放
- HLS 切片
- 静态文件 / 反向代理

### 📦 使用
1. 下载 `nginx-rtmp-httpflv-win64.zip`
2. 解压后直接双击 `nginx.exe`
3. 推流：`ffmpeg -re -i demo.flv -c copy -f flv rtmp://127.0.0.1/live/stream`
4. 播放：
- RTMP：`rtmp://127.0.0.1/live/stream`
- HTTP-FLV：`http://127.0.0.1/live?app=live&stream=stream`
- HLS：`http://127.0.0.1/hls/stream.m3u8`