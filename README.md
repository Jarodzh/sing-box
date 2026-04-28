# Jarodzh 二次开发 - sing-box 一键脚本

> 基于 233boy/sing-box 二次开发

---

## 二次开发内容

### 1. 节点自动命名标准化

搭建节点时自动检测服务器地理位置，按统一格式生成节点名: US-LosAngeles-d5e73f2d

- 国别: Cloudflare trace loc= 自动检测
- 城市/ISP: ipinfo.io /city 或 /org
- UUID前8位: 节点UUID前8字符

目的: 节点名带国别标识后，配合 Sub-Store + convert.min.js 管道，自动归类到对应国家策略组。

### 2. 修复 short_id 为空

原版脚本创建 reality 节点时 short_id 硬编码为空值，部分客户端连接异常。已改为 openssl rand -hex 4 自动生成随机短 ID。

### 3. 修复服务文件缺少 -C 参数

原版脚本服务文件的 ExecStart 缺少 -C /etc/sing-box/conf，导致节点配置未被加载。已修正。

### 4. 位置检测与IP检测解耦

原版 get_ip() 将 IP 获取和位置检测绑定，当 ip 已由安装脚本预置时位置检测被跳过。现已解耦，始终执行。

---

## 安装

wget --no-check-certificate -O install.sh https://raw.githubusercontent.com/Jarodzh/sing-box/feature/country-naming/install.sh && bash install.sh

## 配合 Sub-Store

节点名格式: 国别-城市-UUID前8位 (如 US-LosAngeles-d5e73f2d)
接入管道: 原始节点 → rename.js → convert.min.js → 自动归类到国家策略组

## 文档

原版帮助: https://233boy.com/sing-box/sing-box-script/
