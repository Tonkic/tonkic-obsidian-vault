### 官方
##### 文档
https://sing-box.sagernet.org/
##### 下载
https://github.com/SagerNet/sing-box
### 配置示例
https://github.com/chika0801/sing-box-examples

```bash
nohup ./sing-box run -c config.json &
#短期生效
export http_proxy="http://127.0.0.1:10000"
export https_proxy="http://127.0.0.1:10000"
#长期生效
vi ~/.bashrc
#写入
source ~/.bashrc
```

### 订阅转换
https://v2ray-to-sing-box.pages.dev/