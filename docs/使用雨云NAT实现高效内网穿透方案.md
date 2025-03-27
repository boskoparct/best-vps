# 使用雨云NAT实现高效内网穿透方案

## 1. 项目概述

本文将详细介绍如何利用FRP工具配合雨云NAT服务实现内网穿透。FRP是一款开源的内网穿透工具，支持多种协议穿透，适用于远程办公、游戏联机等场景。

**核心工具**：  
- 服务端：FRP Server (frps)  
- 客户端：FRP Client (frpc)  
- 云服务器：推荐使用[雨云高性能云服务器](https://bit.ly/RainYun)

👉 [【点击查看】2025年最新雨云优惠码及特价云服务器方案汇总](https://bit.ly/RainYun)

## 2. 服务端配置指南

### 2.1 FRP服务端安装

bash
# 创建工作目录
mkdir frp && cd frp
# 下载FRP最新版本（示例版本0.48.0）
wget https://github.com/fatedier/frp/releases/download/v0.48.0/frp_0.48.0_linux_amd64.tar.gz
# 解压安装包
tar -zxvf frp_0.48.0_linux_amd64.tar.gz
cd frp_0.48.0_linux_amd64

### 2.2 配置文件修改

编辑`frps.ini`配置文件：

ini
[common]
bind_port = 7000  # 服务监听端口
token = your_secure_token  # 建议设置复杂token

# 管理后台配置
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = your_password
enable_prometheus = true

**关键参数说明**：
- `bind_port`：必须与客户端配置一致
- `token`：服务端与客户端的通信凭证
- `dashboard`：可视化监控界面

## 3. 服务端运行管理

### 3.1 启动服务

bash
./frps -c ./frps.ini

### 3.2 后台持久化运行

使用screen工具保持服务持续运行：

bash
screen -S frps
./frps -c ./frps.ini
# 退出screen会话：Ctrl+A+D
# 恢复会话：screen -r frps

## 4. 客户端配置详解

### 4.1 Windows客户端配置

编辑`frpc.ini`文件：

ini
[common]
server_addr = your_server_ip  # 雨云服务器IP
server_port = 7000
token = your_secure_token  # 与服务端一致

[game_service]  # 自定义服务名称
local_ip = 127.0.0.1
local_port = 25565  # 本地服务端口
remote_port = 25565  # 外网访问端口

### 4.2 客户端启动

cmd
frpc.exe -c frpc.ini

## 5. 应用场景示例

**游戏服务器穿透**：
- 将本地25565端口(Minecraft默认端口)映射到公网
- 访问方式：`服务器IP:25565`

**远程办公方案**：
- 映射SSH/RDP等端口
- 实现安全远程访问

## 6. 注意事项

1. 防火墙需放行配置的端口
2. 建议定期更新FRP版本
3. 管理后台密码应设置为高强度组合
4. 生产环境建议使用HTTPS加密

如需高性能云服务器支持，推荐使用[雨云云服务器](https://bit.ly/RainYun)，提供稳定可靠的内网穿透基础环境。