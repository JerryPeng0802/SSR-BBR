# 使用亚马逊的AWS来创建一个SS的服务器，并且使用BBR加速

## 1.随便创建一个实例

## 2.使用包安装安装shadowsocks
```
sudo apt update
sudo apt upgrade
sudo apt install shadowsocks
```

## 3.通过apt安装的shadowsocks他的配置文件在/etc/shadowsocks下
```
cd /etc/shadowscd sha   ocks
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
sudo ssserver -c /etc/shadowsocks/config.json -d start
```

## 4.BBR加速

google的一个加速算法，挺好用的
```
sudo apt install wget
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
sudo chmod +x bbr.sh
sudo ./bbr.sh
```

## 5.设置安全组

去实例界面把里面安全组全部打开就行了，包括TCP，UDP，ICMP等等





