# nginx-rtmp-httpflv
============================

开箱即用的 Windows 版 Nginx，已集成：

- **nginx-http-flv-module**（含 RTMP / HLS / HTTP-FLV 全部功能）  
- **OpenSSL**（HTTPS / RTMPS）  
- **PCRE2 + zlib**（完整正则与压缩支持）

下载即用，无需再编译。

---

# 📦 下载
-------

| 文件名 | 说明 |
|---|---|
| `nginx-windows-rtmp-hls-httpflv-x64.zip` | 64 位 Windows 7/10/11/Server |
| `nginx-windows-rtmp-hls-httpflv-x86.zip` | 32 位 Windows 7/10/11/Server |

---

# 🚀 30 秒上手
-------------

1. **解压**  
   把压缩包解压到任意目录，例如 `D:\nginx`。

2. **双击启动**  
   运行 `nginx.exe`，浏览器打开 http://localhost 看到 **Welcome to nginx** 即成功。

3. **推流测试**  
   用 **OBS / FFmpeg** 推流：

   ```bash
   ffmpeg -re -i demo.mp4 -c:v libx264 -c:a aac -f flv \
          rtmp://127.0.0.1/live/stream
   ```

4. **播放地址**  
   - **RTMP**  
     `rtmp://127.0.0.1/live/stream`

   - **HLS**  
     http://localhost/hls/stream.m3u8

   - **HTTP-FLV**  
     http://localhost/live?port=1935&app=live&stream=stream

   （以上地址中 `stream` 可随意替换）

---

# ⚙️ 默认配置摘要
---------------

文件：`conf\nginx.conf`

```nginx
rtmp {
    server {
        listen 1935;
        chunk_size 4096;

        application live {
            live on;
            hls on;
            hls_path temp/hls;
            hls_fragment 3s;
            hls_playlist_length 60s;
        }
    }
}

http {
    server {
        listen 80;

        location / {
            root html;
            index index.html;
        }

        location /live {
            flv_live on;
            add_header Access-Control-Allow-Origin *;
        }

        location /hls {
            types {
                application/vnd.apple.mpegurl m3u8;
                video/mp2t ts;
            }
            alias temp/hls;
            expires -1;
            add_header Cache-Control no-cache;
            add_header Access-Control-Allow-Origin *;
        }
    }
}
```

---

# 🛠️ 常用命令
-----------

| 命令 | 说明 |
|---|---|
| `nginx.exe` | 启动 |
| `nginx.exe -s stop` | 快速停止 |
| `nginx.exe -s reload` | 重载配置 |
| `nginx.exe -t` | 检查配置 |

---

# 📁 目录结构
-----------

```
nginx/
├─ nginx.exe          # 主程序
├─ conf/              # 配置文件
├─ html/              # 默认网页
├─ temp/              # 运行时文件（HLS 切片等）
└─ logs/              # 日志
```

---

# 🔐 HTTPS / RTMPS（可选）
------------------------

把证书 `fullchain.pem` / `privkey.pem` 放到 `conf/ssl/` 目录，  
取消 `nginx.conf` 中 HTTPS 相关注释即可启用 443 端口。

---

# 🐞 故障排查
-----------

1. 80/1935 端口被占用 → 修改 `listen` 端口。  
2. 推流失败 → 检查防火墙、URL、密钥。  
3. 网页无法播放 → 确认 `Access-Control-Allow-Origin *` 已开启（跨域）。

---

# 📄 许可证
---------

Nginx – BSD  
nginx-http-flv-module – BSD  
其他依赖库 – 各自原许可证

---

# 🤝 参与 & 反馈
--------------

- 仓库：https://github.com/SinzMise/nginx-rtmp-httpflv
- Issues / PR 欢迎！

---

Enjoy streaming!