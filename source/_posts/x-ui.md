---
title: X-UI 安装文档
date: 2026-05-27 19:42:09
tags: 
    - x-ui
    - xray
    - 面板
    - 安装指南
categories:
    - 服务器
    - 工具
---

# X-UI 安装文档

> X-UI 是一个支持多协议多用户的 Xray 管理面板，提供网页可视化操作界面，支持流量统计、限制和多种传输协议。内置 HTTPS 支持和自动 SSL 证书续签，适合用于搭建高效的代理服务器管理系统。

## 系统要求

- **操作系统**: CentOS 7+、Ubuntu 16+、Debian 8+
- **CPU架构**: amd64、arm64、arm、386 等
- **内存**: 推荐 512MB 以上
- **存储**: 至少 100MB 可用空间

## 快速安装（推荐）

使用官方安装脚本进行一键安装：

```bash
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

该脚本会自动：
- 下载最新版本
- 安装依赖
- 配置系统服务
- 启动 X-UI

## 手动安装

### 1. 下载安装包

从 [GitHub Releases](https://github.com/vaxilu/x-ui/releases) 下载最新的压缩包，选择对应的 CPU 架构（通常为 `amd64`）。

### 2. 上传到服务器

使用 root 用户登录服务器，将压缩包上传至 `/root/` 目录：

```bash
cd /root/
```

### 3. 安装

执行以下命令进行安装：

```bash
# 清理旧版本
rm x-ui/ /usr/local/x-ui/ /usr/bin/x-ui -rf

# 解压安装包
tar zxvf x-ui-linux-amd64.tar.gz

# 授予执行权限
chmod +x x-ui/x-ui x-ui/bin/xray-linux-* x-ui/x-ui.sh

# 复制可执行文件
cp x-ui/x-ui.sh /usr/bin/x-ui

# 复制系统服务文件
cp -f x-ui/x-ui.service /etc/systemd/system/

# 移动到系统目录
mv x-ui/ /usr/local/

# 刷新系统服务
systemctl daemon-reload

# 启用开机自启
systemctl enable x-ui

# 启动服务
systemctl restart x-ui
```

> **注意**: 如果 CPU 架构不是 `amd64`，请将上述命令中的 `amd64` 替换为对应的架构。

## Docker 安装

### 1. 安装 Docker

```bash
curl -fsSL https://get.docker.com | sh
```

### 2. 创建目录并启动容器

```bash
mkdir x-ui && cd x-ui

docker run -itd --network=host \
    -v $PWD/db/:/etc/x-ui/ \
    -v $PWD/cert/:/root/cert/ \
    --name x-ui --restart=unless-stopped \
    enwaiax/x-ui:latest
```

### 3. 自己构建镜像（可选）

```bash
docker build -t x-ui .
```

## SSL 证书申请

X-UI 内置 SSL 证书申请功能，支持自动申请和续签。使用该功能需要满足以下条件：

### 前置条件

- 拥有 Cloudflare 账户
- 知晓 Cloudflare 注册邮箱
- 获取 Cloudflare Global API Key
- 域名已通过 Cloudflare 解析至当前服务器

### 获取 Cloudflare API Key

1. 登录 [Cloudflare](https://dash.cloudflare.com/)
2. 点击右上角账户头像 → Account Settings
3. 选择 API Tokens 标签
4. 在 API Keys 部分找到 Global API Key
5. 点击 View 按钮查看并复制

### 申请证书

在 X-UI 后台面板中输入以下信息：

- **域名**: 你的域名
- **邮箱**: Cloudflare 注册邮箱
- **API KEY**: Cloudflare Global API Key

系统会自动：
- 使用 DNS API 进行验证
- 从 Let's Encrypt 申请泛域名证书
- 将证书保存至 `/root/cert/` 目录
- 自动续签（无需手动干预）

## 服务管理

### 常用命令

```bash
# 启动服务
systemctl start x-ui

# 停止服务
systemctl stop x-ui

# 重启服务
systemctl restart x-ui

# 查看服务状态
systemctl status x-ui

# 查看日志
journalctl -u x-ui -f

# 禁用开机自启
systemctl disable x-ui

# 启用开机自启
systemctl enable x-ui
```

## 面板访问

安装完成后，X-UI 会在以下地址运行：

- **默认地址**: `http://你的服务器IP:54321`
- **默认用户名**: `admin`
- **默认密码**: `admin`

> **安全建议**: 首次登录后，请立即修改管理员密码

## 从 v2-ui 迁移

如果你之前使用过 v2-ui，可以使用以下命令迁移所有 inbound 账号数据：

```bash
x-ui v2-ui
```

> **注意**: 
> - 迁移只会转移 inbound 账号数据
> - 面板设置和用户名密码不会迁移
> - 迁移成功后请关闭 v2-ui 并重启 x-ui，防止端口冲突

## 主要功能

- ✅ 系统状态监控
- ✅ 多用户多协议支持（VMess、VLESS、Trojan、Shadowsocks 等）
- ✅ 网页可视化配置管理
- ✅ 流量统计和限制
- ✅ 自定义 Xray 配置模板
- ✅ HTTPS 面板访问支持
- ✅ 一键 SSL 证书申请和自动续签
- ✅ Telegram 机器人通知（开发中）
- ✅ 高级配置选项

## 卸载

如需卸载 X-UI，执行以下命令：

```bash
systemctl stop x-ui
systemctl disable x-ui
rm /usr/bin/x-ui -f
rm /etc/systemd/system/x-ui.service -f
rm /usr/local/x-ui/ -rf
systemctl daemon-reload
```

## 常见问题

### Q: 安装脚本执行失败？
A: 确保使用 root 用户权限，并且服务器有网络连接。

### Q: 如何修改默认端口？
A: 在面板后台设置中修改 "面板端口" 后重启服务。

### Q: 如何备份配置？
A: 备份 `/usr/local/x-ui/db/` 目录即可。

### Q: 支持哪些协议？
A: 支持 VMess、VLESS、Trojan、Shadowsocks、Dokodemo-door、Socks、HTTP 等。

## 获取帮助

- **官方 GitHub**: https://github.com/vaxilu/x-ui
- **Issue 反馈**: https://github.com/vaxilu/x-ui/issues
- **Releases**: https://github.com/vaxilu/x-ui/releases

## 更新日志

请查看 [GitHub Releases](https://github.com/vaxilu/x-ui/releases) 了解最新版本和更新信息。
