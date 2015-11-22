# OpenWRT

安装后telnet连接路由器，`passwd`修改密码后再ssh登录。

程序直接在官方源中

## 设置

### PPPoE
```
uci set network.wan.proto=pppoe
uci set network.wan.username='username'
uci set network.wan.password='password'
uci commit network
ifup wan
opkg update
```

### LuCI
```
opkg install luci
/etc/init.d/uhttpd start
/etc/init.d/uhttpd enable
```

### shadowsock

`opkg install shadowsocks-libev`

加密模式优先选择`aes-256-cfb`

设置服务器后，更新排除列表

`wget -O- 'http://ftp.apnic.net/apnic/stats/apnic/delegated-apnic-latest' | awk -F\| '/CN\|ipv4/ { printf("%s/%d\n", $4, 32-log($5)/log(2)) }' > ./ss-ignore.list`

### dnsmasq

vi /etc/dnsmasq.conf
```
#不使用上级DNS解析
#no-resolv
#自定义配置文件解析
conf-dir=/etc/dnsmasq.d/
#全局DNScrypt解析
server=127.0.0.1#5353
cache-size=1024
```

### dnscrypt

`opkg install dnscrypt-proxy`

vi /etc/init.d/dnscrtpt-proxy, add `-T` options open tcp-only

```
/etc/init.d/dnscrypt-proxy enable
/etc/init.d/dnscrypt-proxy start
```
