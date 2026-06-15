# docker-traefik-cloudflare-companion

[![GitHub release](https://img.shields.io/github/v/tag/tiredofit/docker-traefik-cloudflare-companion?style=flat-square)](https://github.com/tiredofit/docker-traefik-cloudflare-companion/releases/latest)
[![Build Status](https://img.shields.io/github/actions/workflow/status/tiredofit/docker-traefik-cloudflare-companionmain.yml?branch=main&style=flat-square)](https://github.com/tiredofit/docker-traefik-cloudflare-companion.git/actions)
[![Docker Stars](https://img.shields.io/docker/stars/tiredofit/traefik-cloudflare-companion.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tiredofit/traefik-cloudflare-companion/)

Translations: [English](../README.md) | [Bahasa Indonesia](README.id.md) | **中文** | [Русский](README.ru.md)

---

## 描述
本项目将构建一个 *Docker 镜像*，该镜像会在检测到新的容器开始运行时，自动更新 *Cloudflare DNS 记录*（CNAME）， specifically designed 用于与 [Traefik Reverse Proxy](https://github.com/traefik/traefik) 配合使用。应用程序会监视 Docker、Docker Swarm 或 Traefik Polling 的事件，以便在 Cloudflare 服务器端进行更新。

## 要求
- **Cloudflare 账户**：活跃的 Global API key 或 Scoped API key。
- **Docker**：访问 `/var/run/docker.sock` 进行容器检测，或 Docker Swarm 集群。
- **Traefik**：v1 或 v2（作为检测标签的代理）。

## 快速开始
```bash
git clone https://github.com/tiredofit/docker-traefik-cloudflare-companion.git
cd docker-traefik-cloudflare-companion
cp .env.example .env

# 编辑 .env 文件以您的喜好进行配置，至少填写 CF_TOKEN。
nano .env

# 通过 Docker Compose 运行容器
docker compose up -d
```

## 环境变量
配置详情（包括 *Docker 选项*、*Cloudflare 选项* 和 *Traefik 选项*）可以通过复制并参考根目录中的内置 **`.env.example`** 文件来查看。

## 项目结构
项目的主要结构包括：
- `docs/` : 特定文档文件夹，包括架构、测试、故障排除等。
- `install/` : s6-overlay 脚本和配置文件，以及 Docker Alpine 内部服务初始化。
- `examples/` : 使用 `docker-compose.yml` 的示例。
- `Dockerfile` : 用于此 Python Cloudflare 脚本的 Alpine Linux 容器构建蓝图。

## 运行测试
可以使用干运行模式在不改变 Cloudflare 数据的情况下测试此应用程序：

```bash
# 在您的 .env 文件中更改以下设置以启用：
DRY_RUN=TRUE

# 重启并监控日志
docker compose up -d
docker compose logs -f
```

*(重要：当脚本正确读取 DNS 路由后，请记得将 `DRY_RUN` 设置为 `FALSE`)*。

## 文档
详细文档已分离以避免干扰。所有其他信息都在 `/docs` 文件夹中可用：
- [概览](overview.md)
- [架构与发现](architecture.md)
- [数据库与状态](database.md)
- [API 集成](api.md)
- [部署](deployment.md)
- [测试](testing.md)
- [故障排除](troubleshooting.md)

## 贡献者
- [Dave Conroy](http://github/tiredofit/) - *维护者* & *赞助者*
