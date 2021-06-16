# 使用亚马逊的AWS来创建一个SS的服务器，并且使用BBR加速

## 1.随便创建一个实例

## 2.使用包安装安装shadowsocks
```
sudo apt update
sudo apt upgrade
sudo apt install shadowsocks-libev
```

## 3.通过apt安装的shadowsocks他的配置文件在/etc/shadowsocks下
```
cd /etc/shadowsocks-libev
sudo vim config.json

```
配置内容改为下面的

```
{
    "server":"0.0.0.0",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"password",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "prefer_ipv6": false
}

```
server必须是0.0.0.0，否则连不上，端口密码啥的自己改，fase_open和prefer_ipv6这两个参数写为false就行了

开启SSR服务
```
sudo systemctl start shadowsocks-libev
```

开启SSR的开机自启动
```
sudo systemctl enable shadowsocks-libev
```

查看是否设置设置成功

```
sudo systemctl list-unit-files | grep shadowsocks

```
## 4.BBR加速

google的一个加速算法，挺好用的
```
sudo apt install wget
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
sudo chmod +x bbr.sh
sudo ./bbr.sh
```
实际上最新的ubuntu，不需要下载这个脚本可以使用如下方法：

修改环境变量
```
sudo bash -c 'echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf'

sudo bash -c 'echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf'

```
保存配置生效
```
sysctl -p
```

查看是否开启bbr

```
sysctl net.ipv4.tcp_available_congestion_control
```
如果返回的是
```
net.ipv4.tcp_available_congestion_control = reno cubic bbr
```
那就可以了
## 5.设置安全组

去实例界面把里面安全组全部打开就行了，包括TCP，UDP，ICMP等等


## 6.谷歌学术无法访问的问题，

首先要确保当前vps支持IPv6，然后打开https://github.com/popcorner/cernet-ipv6-hosts ，找到谷歌学术，这里
下载下来在搜索比较好。

sudo vim /etc/hosts

将里面关于scholar的内容复制在后面就行了


