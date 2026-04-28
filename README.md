# Jarodzh 二次开发 - sing-box 一键脚本

> 基于 [233boy/sing-box](https://github.com/233boy/sing-box) 二次开发

---

## 二次开发内容

### 1. 节点自动命名标准化

搭建节点时自动检测服务器地理位置，按统一格式生成节点名: `US-LosAngeles-d5e73f2d`

- **国别**: Cloudflare trace `loc=` 自动检测
- **城市/ISP**: ipinfo.io `/city` 或 `/org` 自动检测
- **UUID前8位**: 节点 UUID 的前 8 字符

目的: 节点名带国别标识后，配合 Sub-Store + convert.min.js 管道，自动归类到对应国家策略组。

### 2. 修复 short_id 为空

原版脚本创建 reality 节点时 `short_id` 硬编码为空数组 `[""]`，部分客户端连接异常。已改为 `openssl rand -hex 4` 自动生成随机短 ID。

### 3. 修复服务文件缺少 -C 参数

原版脚本服务文件的 `ExecStart` 缺少 `-C /etc/sing-box/conf`，导致节点配置文件未被加载。已修正为:

```
ExecStart=/etc/sing-box/bin/sing-box run -c /etc/sing-box/config.json -C /etc/sing-box/conf
```

### 4. 位置检测与 IP 检测解耦

原版 `get_ip()` 将 IP 获取和位置检测绑定在一起，当 IP 已由安装脚本预置时位置检测被跳过，节点名为空。现已解耦，位置检测始终执行。

---

## 安装

```bash
wget --no-check-certificate -O install.sh https://raw.githubusercontent.com/Jarodzh/sing-box/feature/country-naming/install.sh && bash install.sh
```

---

## 配合 Sub-Store 使用

节点名格式: `国别-城市-UUID前8位` (如 `US-LosAngeles-d5e73f2d`)

接入管道: `原始节点 → rename.js → convert.min.js → 自动归类到国家策略组`

---

## 原作者功能 (233boy/sing-box)

以下为原版脚本的全部特性：

### 特点

- 快速安装，无敌好用，零学习成本
- 自动化 TLS，简化所有流程
- 兼容 sing-box 命令
- 强大的快捷参数
- 支持所有常用协议
- 一键添加 VLESS-REALITY (默认)
- 一键添加 TUIC / Trojan / Hysteria2 / AnyTLS
- 一键添加 Shadowsocks 2022
- 一键添加 VMess-(TCP/HTTP/QUIC)
- 一键添加 VMess / VLESS / Trojan (WS/H2/HTTPUpgrade)-TLS
- 一键启用 BBR
- 一键更改伪装网站
- 一键更改 (端口/UUID/密码/域名/路径/加密方式/SNI/等...)

### 设计理念

设计理念为：**高效率，超快速，极易用**

脚本基于作者的自身使用需求，以 **多配置同时运行** 为核心设计

专门优化了添加、更改、查看、删除这四项常用功能

你只需要一条命令即可完成添加、更改、查看、删除等操作

### 使用帮助

安装完成后输入 `sing-box help` 查看完整帮助，或 `sb help`

---

## 文档

- 原版安装及使用: https://233boy.com/sing-box/sing-box-script/
- 反馈问题: https://github.com/233boy/sing-box/issues
