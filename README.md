# ssr-vpn
vpn安装
1.首先安装docker
Linux安装最新版Docker完整教程
一、安装前准备工作
#1.1 查看服务器系统版本以及内核版本
cat /etc/redhat-release
查看服务器内核版本
uname -r
# 安装依赖包
yum install -y yum-utils device-mapper-persistent-data lvm2
# docker-ce安装 社区版
yum install -y docker
启动docker并设置开机自启
# 启动docker命令
systemctl start docker
# 设置开机自启命令
systemctl enable docker
#查看docker版本命令
docker version
# docker 安装完成后
#安装ssr
# 拉取镜像
$ docker pull teddysun/shadowsocks-r
# 首先必须在host中创建一个配置文件： /etc/shadowsocks-r/config.json
{
    "server":"0.0.0.0",
    "server_ipv6":"::",
    "server_port":9000,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"password0",
    "timeout":120,
    "method":"aes-256-cfb",
    "protocol":"origin",
    "protocol_param":"",
    "obfs":"plain",
    "obfs_param":"",
    "redirect":"",
    "dns_ipv6":false,
    "fast_open":true,
    "workers":1
}
# 新建免流/jsdata/ssr/config.json
{
  "server": "0.0.0.0",
  "server_ipv6": "::",
  "server_port": 80,
  "local_address": "127.0.0.1",
  "local_port": 1080,
  "password": "123456",
  "timeout": 120,
  "method": "chacha20",
  "protocol": "auth_chain_a",
  "protocol_param": "",
  "obfs": "http_simple",
  "obfs_param": "",
  "redirect": "",
  "dns_ipv6": false,
  "fast_open": true,
  "workers": 1
}
# 参数解释：
#server_port：SSR端口，建议80,443
#password：SSR连接密码
#method：加密方式（不重要），常见的加密方式自行谷歌
#protocol：协议（不重要），常见的协议自行谷歌
#obfs：混淆方式（重要），必须是http_simple
# 运行启动搭建完后
docker run -d --name js-ssr --restart=always \
    -p 80:80 -p 80:80/udp \
    -v /jsdata/ssr/config.json:/etc/shadowsocks-r/config.json \
    teddysun/shadowsocks-r
    
#Android：shadowsocksr-android,https://github.com/shadowsocksrr/shadowsocksr-android/releases
#IOS：Shadowrocket,https://apps.apple.com/us/app/shadowrocket/id932747118
#Windows：shadowsocksr-csharp,https://github.com/shadowsocksrr/shadowsocksr-csharp/releases
# 非docker版VPN
# 用pip安装ss服务
yum install python-setuptools && easy_install pip
# 安装shadowsocks
pip install shadowsocks
# 启动ssserver
ssserver -p 443 -k MyPass -m rc4-md5 -d start
#ssserver命令中 443是指端口 MyPass是指密码 rc4-md5是指加密方式，这些小伙伴们都可以自定义修改
# 停止ss服务可用 
ssserver -d stop
#需要其余加密执行如下命令
yum install python–m2crypto
