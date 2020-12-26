# 在Ubuntu20.04使用SSR和Proxychains4  

## 使用apt直接安装Proxychains4和shadowsocks-libv

```
sudo apt update
sudo apt install proxychains4
sudo apt install shadowsocks-libev
```
## 根据自己服务器的信息修改/etc/shadowsocks-libev/config.json 

## 修改/etc/proxychains4.conf
将最后一行的“ socks4 127.0.0.1 9050 ”，更改为 sock5 127.0.0.1 1080 (1080是我的本地监听端口，一般是1080，若不是，修改为你自己的本地端口，这里是根据你上面信息来的)

## 使用ss-loacl启动ssr
```
ss-local -c /etc/shadowsocks-libev/config.json
```

##  测试一下
```
proxychains4 wget https://www.google.com
```
能把网页下载下来说明就行了，以后使用到代理的是否记得加   proxychains4   前缀

当然这个前缀过长可以通过终端的配置文件修改一下

```
vim .bashrc
在文件末尾添加 alias pc4=proxychains4
source .bashrc
```
以后就可以通过pc4来代替上面的
