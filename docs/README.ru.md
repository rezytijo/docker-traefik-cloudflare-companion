# docker-traefik-cloudflare-companion

[![GitHub release](https://img.shields.io/github/v/tag/tiredofit/docker-traefik-cloudflare-companion?style=flat-square)](https://github.com/tiredofit/docker-traefik-cloudflare-companion/releases/latest)
[![Build Status](https://img.shields.io/github/actions/workflow/status/tiredofit/docker-traefik-cloudflare-companionmain.yml?branch=main&style=flat-square)](https://github.com/tiredofit/docker-traefik-cloudflare-companion.git/actions)
[![Docker Stars](https://img.shields.io/docker/stars/tiredofit/traefik-cloudflare-companion.svg?style=flat-square&logo=docker)](https://hub.docker.com/r/tiredofit/traefik-cloudflare-companion/)

Translations: [English](../README.md) | [Bahasa Indonesia](README.id.md) | [中文](README.zh.md) | **Русский**

---

## Описание
Этот проект собирает *Docker-образ*, который автоматически обновляет *DNS-записи Cloudflare* (CNAME) при обнаружении нового запущенного контейнера, специально предназначенный для использования с [Traefik Reverse Proxy](https://github.com/traefik/traefik). Приложение отслеживает события Docker, Docker Swarm или Traefik Polling, чтобы вносить изменения на стороне сервера Cloudflare.

## Требования
- **Cloudflare-аккаунт**: активный глобальный API-ключ или ограниченный API-ключ.
- **Docker**: доступ к `/var/run/docker.sock` для обнаружения контейнеров, или кластер Docker Swarm.
- **Traefik**: v1 или v2 (в качестве обратного прокси, метки которого обнаруживаются).

## Быстрый старт
```bash
git clone https://github.com/tiredofit/docker-traefik-cloudflare-companion.git
cd docker-traefik-cloudflare-companion
cp .env.example .env

# Отредактируйте файл .env согласно вашим предпочтениям, минимум нужно указать CF_TOKEN.
nano .env

# Запуск контейнера через Docker Compose
docker compose up -d
```

## Переменные окружения
Подробные настройки (включая *Docker-опции*, *Cloudflare-опции* и *Traefik-опции*) можно найти, скопировав и обратив внимание на встроенный **`.env.example`** файл в корневой директории.

## Структура проекта
Основная структура проекта включает:
- `docs/` : Папка с документацией по архитектуре, тестированию, устранению неполадок и т.д.
- `install/` : Скрипты и конфигурационные файлы s6-overlay, а также внутренняя инициализация сервиса Alpine Docker.
- `examples/` : Примеры использования с `docker-compose.yml`.
- `Dockerfile` : Шаблон для создания Alpine Linux-контейнера для этого Python-скрипта Cloudflare.

## Тестирование
Эту программу можно протестировать без риска изменения данных Cloudflare (*dry run*):

```bash
# В файле .env измените на:
DRY_RUN=TRUE

# Перезапуск и мониторинг логов
docker compose up -d
docker compose logs -f
```

*(Важно: после того как скрипт правильно прочитал маршрутизацию DNS, не забудьте вернуть `DRY_RUN` в `FALSE`)*.

## Документация
Подробная документация разделена, чтобы избежать шума. Всю дополнительную информацию можно найти в папке `/docs`:
- [Обзор](overview.md)
- [Архитектура и обнаружение](architecture.md)
- [База данных и состояние](database.md)
- [Интеграция API](api.md)
- [Развёртывание](deployment.md)
- [Тестирование](testing.md)
- [Устранение неполадок](troubleshooting.md)

## Участники
- [Dave Conroy](http://github/tiredofit/) - *Поддерживатель* & *Спонсор*
