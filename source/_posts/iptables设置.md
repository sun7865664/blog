### 南通漏洞修复

##### 一、数据库漏洞

###### 1、禁用数据库1521端口，并限定203，205及本地可以访问

```bash
iptables -I INPUT -p tcp --dport 1521 -j DROP
iptables -I INPUT -s 2.82.64.205 -p TCP --dport 1521 -j ACCEPT
iptables -I INPUT -s 2.82.64.203 -p TCP --dport 1521 -j ACCEPT
iptables -I INPUT -s 127.0.0.1 -p TCP --dport 1521 -j ACCEPT
```

###### 2、禁用数据库的5500端口，并限定203，205及本地可以访问

```bash
iptables -I INPUT -p tcp --dport 5500 -j DROP
iptables -I INPUT -s 2.82.64.205 -p TCP --dport 5500 -j ACCEPT
iptables -I INPUT -s 2.82.64.203 -p TCP --dport 5500 -j ACCEPT
iptables -I INPUT -s 127.0.0.1 -p TCP --dport 5500 -j ACCEPT
```

##### 二、traceroute探测

1、禁用icmp协议

```bash
iptables -A INPUT -p ICMP --icmp-type time-exceeded -j DROP
iptables -A OUTPUT -p ICMP --icmp-type time-exceeded -j DROP
```

##### 三、iptables查询、删除规则

###### 1、查询iptables规则及行号

```bash
iptables -nL --line-number
```

###### 2、根据行号删除规则

```
iptables -D INPUT 1
```



