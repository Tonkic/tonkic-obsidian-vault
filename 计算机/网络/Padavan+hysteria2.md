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