https://github.com/apernet/hysteria/releases
下载hysteria-linux-mipsle
```shell
chmod +x /opt/bin/hysteria
sh /etc/storage/hy2_tproxy.sh
```
### 前台运行
/opt/bin/hysteria client --config /etc/storage/hy2.yaml

### 检查
检查进程
ps | grep hysteria
检查端口 
netstat -tuln | grep 1090

- **jsDelivr (Fastly):** `https://fastly.jsdelivr.net/gh/hiboyhiboy/opt-file@master/Advanced_Extensions_hysteriaasp`
放到/opt/app/hysteria/
下载下来的文件加个点Advanced_Extensions_hysteria.asp