cd /etc/netplan/
vi这个目录下的文件
### 示例
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      addresses: [192.168.1.100/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```
应用配置 sudo netplan apply

如果配置文件如下，见[[#^0e6bac|NetworkManager]]
```yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
```

# NetworkManager

^0e6bac

sudo nm-connection-editor可视化更改