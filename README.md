#### caddy 代理 websocket 流量转发给 v2ray 处理 

vmess over wss `ss vmess 这些自发明的翻墙协议最终都跑在tls里面:)`

选择 caddy 因为支持自动续签证书，请先解析自己域名到部署服务的 ip 地址生效后再使用，

默认占用 80 433 端口, 要使用其他端口，自行修改 Caddyfile 配置

安装 [docker-ce](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

安装 [docker-compose](https://docs.docker.com/compose/install/)
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

替换 uuid
```
git clone https://github.com/luoqeng/caddy-v2ray-docker.git && cd caddy-v2ray-docker

UUID=$(cat /proc/sys/kernel/random/uuid) && echo ${UUID}

sed -i "s/uuid/${UUID}/" ./v2ray/config.json
```

替换域名
```
DOMAIN=www.xxxx.com

sed -i "s/your.domain/${DOMAIN}/" ./caddy/Caddyfile
```

替换 https 代理默认账号密码 admin pass123456 `支持 h2 tls1.3`
```
HTTPS_USER=ubuntu
sed -i "s/admin/${HTTPS_USER}/" ./caddy/Caddyfile

HTTPS_PASSWD=pass2048
sed -i "s/pass123456/${HTTPS_PASSWD}/" ./caddy/Caddyfile
```

默认页面
```
echo 'hello' > ./caddy/srv/index.html
```

启动 caddy + v2ray
```
sudo docker-compose up -d
```

v2ray [客户端](https://www.v2ray.com/awesome/tools.html)配置
```
sed -i "s/uuid/${UUID}/" ./v2ray/client-simple-config.json
sed -i "s/your.domain/${DOMAIN}/" ./v2ray/client-simple-config.json
```

透明代理推荐使用 koolshare x64 固件离线安装科学上网
 - https://github.com/hq450/fancyss_history_package/tree/master/fancyss_X64
 - https://firmware.koolshare.cn/LEDE_X64_fw867

参考
 - https://gist.github.com/dcb9/1e0f0346400e42fb4d03ead124da1658
 - https://github.com/nanking/docker-caddy-v2ray
