# iptables

## 不同ip端口转发

iptables -t nat -A PREROUTING -p tcp --dport 30022 -j DNAT --to-destination 192.168.31.88:22

iptables -t nat -A POSTROUTING -d 192.168.31.88 -p tcp --dport 22 -j SNAT --to 192.168.31.8

iptables-save

echo 1 > /proc/sys/net/ipv4/ip_forward

## 查看nat表

iptables -nvL --line-number

_加上-t nat，查看nat表_

iptables -L -t nat --line-number

## 删除
iptables -D POSTROUTING 1 -t nat
