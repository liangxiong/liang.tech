- [参考文档](https://yq.aliyun.com/articles/243455?spm=5176.100239.blogcont243456.10.57c95938a8LsYz)

- 参数：
  - ss 8 -t 3
  - c copy
  - s 分辨率控制
  - g 关键帧间隔控制
  - re 限制输出速率，按照帧率输出
  - b:v 1500k  输出码率
  - bufsize 1500k  buffer 码率
  - maxrate 2500k  最大码率
  - r 设置帧率

- 案例
  - ffmpeg -y -i 'encode.mp4' -b:v 1500k -bufsize 1500k -maxrate 2500k  -r 25 t.mp4


- 视频格式
  - 1080p =1920x1080
  - 720p = 1280x720    
  - 标清 = 448x336，512x288
  - 高清 = 576x432，672x378
  - 超清 = 1104x622

- youtube 默认格式:
  - 分辨率：1920x1080
  - 视频比特率范围：3000 - 6000 Kbps

![架构图](https://github.com/liangxiong/liang.tech/blob/master/多媒体/res/video_format.png)
