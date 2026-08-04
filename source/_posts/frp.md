---
title: frp
date: 2026-05-26 22:06:22
tags: 
    - frp
    - frpc
    - frps
    - 内网穿透
categories: 
    - 服务器
---

# 服务端部署
## 下载
```sh
wget https://github.com/fatedier/frp/releases/download/v0.69.0/frp_0.69.0_linux_amd64.tar.gz
```
## 安装
```sh
tar -zxvf frp_0.69.0_linux_amd64.tar.gz
cd frp_0.69.0_linux_amd64
vim frps.toml

\\
bindPort = 7000

[auth]
method = "token"
token = "thisiskukuboy"

[webServer]
addr = "0.0.0.0"
port = 7500
user = "admin"
password = "12345"
\\
```
## 编辑配置文件

Here are the specific operation steps:

Install systemd

If systemd is not yet installed on your Linux server, you can use package managers like yum (for CentOS/RHEL) or apt (for Debian/Ubuntu) to install it:

### Install systemd using yum (CentOS/RHEL)
yum install systemd

### Install systemd using apt (Debian/Ubuntu)
apt install systemd
Create frps.service file

Use a text editor (like vim) to create a frps.service file in the /etc/systemd/system directory to configure the frps service.
```
$ sudo vim /etc/systemd/system/frps.service
```
Write the content:
```
[Unit]
# Service name, customizable
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
# Command to start frps, modify to your frps installation path
ExecStart = /path/to/frps -c /path/to/frps.toml

[Install]
WantedBy = multi-user.target
Use systemd commands to manage frps service
```

### Start frp
sudo systemctl start frps
### Stop frp
sudo systemctl stop frps
### Restart frp
sudo systemctl restart frps
### Check frp status
sudo systemctl status frps
Set frps to auto-start on boot

sudo systemctl enable frps

# window客户端安装

## 下载

网址下载
https://github.com/fatedier/frp/releases

## 安装
1. 解压
2. 配置
编辑frpc.toml文件
```
[common]
server_addr = 公网服务器IP  # 例如：123.123.123.123
server_port = 7000          # 与FRPS的bind_port一致
token = 123456              # 认证令牌（可选）

[web]                       # 自定义服务名称
type = http                 # 协议类型（http/tcp/udp）
local_port = 80             # 内网服务端口
local_ip = 127.0.0.1        # 内网服务IP
remote_port = 8080          # 公网服务器暴露的端口

```
1. 启动
```
frpc.exe -c frpc.toml
```