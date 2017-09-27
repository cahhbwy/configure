# network configuration and tips

## ssh登陆不上去

1. 检查用户名和密码

2. 检查sshd_config的允许登陆方式

3. 检查hosts.deny和iptables、denyhosts
```bash
sudo iptables -F
sudo service denyhosts stop
sudo denyhosts --purge-all
sudo service denyhosts start
```

## iptables

### 不同ip端口转发
```bash
iptables -t nat -A PREROUTING -p tcp --dport 30022 -j DNAT --to-destination 192.168.31.88:22
iptables -t nat -A POSTROUTING -d 192.168.31.88 -p tcp --dport 22 -j SNAT --to 192.168.31.8
iptables-save
echo 1 > /proc/sys/net/ipv4/ip_forward
```

### 查看nat表
```bash
iptables -nvL --line-number
```

_加上-t nat，查看nat表_
```bash
iptables -L -t nat --line-number
```

### 删除
```bash
iptables -D POSTROUTING 1 -t nat
```

### 清空
```bash
iptables -F
```
