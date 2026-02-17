# ByteDesk Janus Docker Image

[![Docker Hub](https://img.shields.io/docker/v/bytedesk/janus?label=Docker%20Hub)](https://hub.docker.com/r/bytedesk/janus)
[![Docker Pulls](https://img.shields.io/docker/pulls/bytedesk/janus)](https://hub.docker.com/r/bytedesk/janus)
[![License](https://img.shields.io/github/license/Bytedesk/bytedesk-janus)](LICENSE)

Janus Gateway Docker image for ByteDesk, based on Ubuntu 22.04 LTS.

**Language:** [English](README.md) | [中文](README.zh.md)

## 📑 Table of Contents

- [Features](#features)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Image Build and Release](#image-build-and-release)
- [Environment Variables](#environment-variables)
- [Ports](#ports)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Features

- ✅ Janus Gateway `v1.4.0` (built from source)
- ✅ Ubuntu `22.04` base image
- ✅ Built-in dependencies for WebRTC stack
  (`libwebsockets`, `libsrtp`, `libnice`, `usrsctp`)
- ✅ Janus built with `--enable-post-processing`
- ✅ `FFmpeg` included for post-processing workflows
- ✅ Multi-architecture image publishing via GitHub Actions
- ✅ Docker Hub and Aliyun registry publishing
- ✅ Single-process runtime (Nginx removed, Janus only)
- ❌ RabbitMQ/MQTT/Unix sockets are disabled in current build flags

## Quick Start

### Pull image

```bash
# Docker Hub
docker pull bytedesk/janus:latest

# Aliyun registry (recommended in mainland China)
docker pull registry.cn-hangzhou.aliyuncs.com/bytedesk/janus:latest
```

### Run container

The image starts Janus directly (`CMD ["janus"]`).

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

## Configuration

Janus config files are generated during build by `make configs`
and installed under `/usr/local/etc/janus`.

To customize configuration, mount your config directory:

```bash
docker run -d \
  --name janus \
  -v $(pwd)/janus-conf:/usr/local/etc/janus \
  -p 8088:8088 \
  -p 8188:8188 \
  -p 10000-10200:10000-10200/udp \
  bytedesk/janus:latest
```

Check logs:

```bash
docker logs -f janus
```

## Image Build and Release

This repository uses GitHub Actions workflow at `.github/workflows/janus-docker.yml`.

- Tag trigger: `janus-v*` (example: `janus-v1.4.0`)
- Manual trigger: `workflow_dispatch` with `version` and `push_to_registry`
- Registries:
  - `bytedesk/janus`
  - `registry.cn-hangzhou.aliyuncs.com/bytedesk/janus`

Example release tag:

```bash
git tag janus-v1.4.0
git push origin janus-v1.4.0
```

## Environment Variables

- `TZ`: Container timezone (default: `Asia/Shanghai`)

Most Janus runtime behavior is controlled by files in `/usr/local/etc/janus`.

## Ports

Janus ports depend on your `janus.jcfg` and plugin configs. Common mappings:

- `8088`/TCP: Janus HTTP API
- `8188`/TCP: Janus WebSocket API
- `10000-10200`/UDP: RTP media ports (example range)

Adjust these to match your deployed Janus configuration.

## Troubleshooting

- Container exits immediately: inspect `docker logs janus` for config errors.
- API not reachable: verify mapped ports and firewall/security group rules.
- Media issues: ensure UDP media port range is open and consistent with Janus config.
- Custom config not applied: confirm mount target is `/usr/local/etc/janus`.

## Contributing

Issues and pull requests are welcome.

- Repository: [Bytedesk/bytedesk-janus](https://github.com/Bytedesk/bytedesk-janus)
- Docker Hub: [bytedesk/janus](https://hub.docker.com/r/bytedesk/janus)

## License

This project is licensed under the terms in [LICENSE](LICENSE).
