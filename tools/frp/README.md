---
date: 2022-09-11T11:59:36+08:00  # 创建日期
author: "Rustle Karl"  # 作者

title: "frp 内网穿透服务端配置教程"  # 文章标题
url:  "posts/aws/tools/frp/README"  # 设置网页永久链接
tags: [ "aws", "readme" ]  # 标签
categories: [ "AWS 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

## 安装

> https://github.com/fatedier/frp/releases

### 下载

```bash
mkdir Downloads
```

```bash
cd Downloads
```

```bash
wget https://github.com/fatedier/frp/releases/download/v0.44.0/frp_0.44.0_linux_amd64.tar.gz -O frp.tar.gz
```

### 解压

```bash
tar -zxvf frp.tar.gz
```

```bash
cd 解压后的目录
```

### 修改配置

```bash
apt install -y micro
```

```bash
micro frps.ini
```

```bash
[common]
# 监听端口
bind_port = 7000 

# 授权码
token = 52010

# 管理后台端口
dashboard_port = 7500 

# 管理后台用户名和密码
dashboard_user = admin 
dashboard_pwd = admin 

log_file = /var/log/frps.log 
log_level = info 
log_max_days = 3
```

### 复制文件

```bash
sudo mkdir -p /etc/frp
sudo cp frps.ini /etc/frp
sudo cp frps /usr/bin
```

### 创建服务

```bash
micro frps.service
```

```bash
[Unit]
Description=Frp Server Service
After=network.target

[Service]
Type=simple
User=nobody
Restart=on-failure
RestartSec=5s
ExecStart=/usr/bin/frps -c /etc/frp/frps.ini
LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target
```

### 启动服务

```bash
sudo cp frps.service /usr/lib/systemd/system/
sudo systemctl enable frps
sudo systemctl start frps
```

### 测试是否成功

```bash
http localhost:7500
```

```bash
http localhost:7000
```

## 客户端配置教程

见 [Raspberrypi 学习笔记](https://github.com/studying-notes/raspberrypi-notes)
