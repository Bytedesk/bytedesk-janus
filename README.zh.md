# 微语 Janus Docker 镜像

[![Docker Hub](https://img.shields.io/docker/v/bytedesk/janus?label=Docker%20Hub)](https://hub.docker.com/r/bytedesk/janus)
[![Docker Pulls](https://img.shields.io/docker/pulls/bytedesk/janus)](https://hub.docker.com/r/bytedesk/janus)
[![License](https://img.shields.io/github/license/Bytedesk/bytedesk-janus)](LICENSE)

面向 ByteDesk 的 Janus Gateway Docker 镜像，基于 Ubuntu 22.04 LTS。

**语言 / Language:** [中文](README.zh.md) | [English](README.md)

## 📑 目录

- [功能特性](#功能特性)
- [快速开始](#快速开始)
- [配置说明](#配置说明)
- [镜像构建与发布](#镜像构建与发布)
- [环境变量](#环境变量)
- [端口说明](#端口说明)
- [常见问题](#常见问题)
- [贡献](#贡献)
- [许可证](#许可证)

## 功能特性

- ✅ Janus Gateway `v1.4.0`（源码编译）
- ✅ 基于 Ubuntu `22.04`
- ✅ 内置 WebRTC 核心依赖（`libwebsockets`、`libsrtp`、`libnice`、`usrsctp`）
- ✅ Janus 开启 `--enable-post-processing`
- ✅ 内置 `FFmpeg`，便于后处理流程
- ✅ GitHub Actions 支持多架构镜像发布
- ✅ 同时发布到 Docker Hub 和阿里云镜像仓库
- ✅ 运行时单进程（已移除 Nginx，仅运行 Janus）
- ❌ 当前构建参数中已禁用 RabbitMQ / MQTT / Unix sockets

## 快速开始

### 拉取镜像

```bash
# Docker Hub
docker pull bytedesk/janus:latest

# 阿里云镜像仓库（中国大陆推荐）
docker pull registry.cn-hangzhou.aliyuncs.com/bytedesk/janus:latest
```

### 启动容器

镜像启动命令为 `CMD ["janus"]`，容器内仅运行 Janus。

```bash
docker run -d \
  --name janus \
  -p 8088:8088 \
  -p 8188:8188 \
  -p 10000-10200:10000-10200/udp \
  -e TZ=Asia/Shanghai \
  --restart=unless-stopped \
  bytedesk/janus:latest
```

## 配置说明

Janus 配置由构建阶段 `make configs` 生成，并安装在 `/usr/local/etc/janus`。

如需自定义配置，可将本地目录挂载到该路径：

```bash
docker run -d \
  --name janus \
  -v $(pwd)/janus-conf:/usr/local/etc/janus \
  -p 8088:8088 \
  -p 8188:8188 \
  -p 10000-10200:10000-10200/udp \
  bytedesk/janus:latest
```

查看运行日志：

```bash
docker logs -f janus
```

## 镜像构建与发布

仓库使用 GitHub Actions 工作流：`.github/workflows/janus-docker.yml`。

- 标签触发：`janus-v*`（例如 `janus-v1.4.0`）
- 手动触发：`workflow_dispatch`（支持 `version`、`push_to_registry`）
- 发布仓库：
  - `bytedesk/janus`
  - `registry.cn-hangzhou.aliyuncs.com/bytedesk/janus`

发布示例：

```bash
git tag janus-v1.4.0
git push origin janus-v1.4.0
```

## 环境变量

- `TZ`：容器时区（默认值：`Asia/Shanghai`）

Janus 的大多数运行行为由 `/usr/local/etc/janus` 下配置文件控制。

## 端口说明

Janus 端口最终以 `janus.jcfg` 与各插件配置为准。常见端口如下：

- `8088`/TCP：Janus HTTP API
- `8188`/TCP：Janus WebSocket API
- `10000-10200`/UDP：RTP 媒体端口（示例范围）

请根据你实际部署的 Janus 配置调整端口映射。

## 服务器

- [Bytedesk](https://github.com/Bytedesk/bytedesk)

## 开源Demo + SDK

| Project     | Description           | Forks          | Stars             |
|-------------|-----------------------|----------------|-------------------|
| [iOS](https://github.com/bytedesk/bytedesk-swift) | iOS  | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-swift) | ![GitHub Repo stars](https://img.shields.io/github/stars/Bytedesk/bytedesk-swift)                 |
| [Android](https://github.com/bytedesk/bytedesk-android) | Android | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-android) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-android)  |
| [Flutter](https://github.com/bytedesk/bytedesk-flutter) | Flutter | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-flutter)| ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-flutter) |
| [UniApp](https://github.com/bytedesk/bytedesk-uniapp) | Uniapp | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-uniapp) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-uniapp) |
| [Web](https://github.com/bytedesk/bytedesk-web) | Vue/React/Angular/Next.js/JQuery/... | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-web) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-web) |
| [Wordpress](https://github.com/bytedesk/bytedesk-wordpress) | Wordpress | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-wordpress) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-wordpress) |
| [Woocommerce](https://github.com/bytedesk/bytedesk-woocommerce) | woocommerce | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-woocommerce) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-woocommerce) |
| [Magento](https://github.com/bytedesk/bytedesk-magento) | Magento | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-magento) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-magento) |
| [Prestashop](https://github.com/bytedesk/bytedesk-prestashop) | Prestashop | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-prestashop) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-prestashop) |
| [Shopify](https://github.com/bytedesk/bytedesk-shopify) | Shopify | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-shopify) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-shopify) |
| [Opencart](https://github.com/bytedesk/bytedesk-opencart) | Opencart | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-opencart) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-opencart) |
| [Laravel](https://github.com/bytedesk/bytedesk-laravel) | Laravel | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-laravel) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-laravel) |
| [Django](https://github.com/bytedesk/bytedesk-django) | Django | ![GitHub forks](https://img.shields.io/github/forks/bytedesk/bytedesk-django) | ![GitHub Repo stars](https://img.shields.io/github/stars/bytedesk/bytedesk-django) |

## 常见问题

- 容器启动后退出：先查看 `docker logs janus` 定位配置错误。
- API 无法访问：检查端口映射、防火墙和安全组规则。
- 音视频媒体异常：确认 UDP 媒体端口范围已放通且与 Janus 配置一致。
- 自定义配置未生效：确认挂载目标路径为 `/usr/local/etc/janus`。

## 贡献

欢迎提交 Issue 和 Pull Request。

- 仓库地址：[Bytedesk/bytedesk-janus](https://github.com/Bytedesk/bytedesk-janus)
- Docker Hub：[bytedesk/janus](https://hub.docker.com/r/bytedesk/janus)

## 许可证

本项目采用 [LICENSE](LICENSE) 中定义的许可协议。
