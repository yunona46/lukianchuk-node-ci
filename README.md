# Node.js CI/CD Project with Docker & AWS EC2

Автор: **yunona46**

## 📋 Опис

Node.js застосунок з повною автоматизацією CI/CD через GitHub Actions. При кожному push автоматично збирається Docker образ, завантажується в Docker Hub і деплоїться на AWS EC2.

## 🚀 CI/CD Pipeline

### Автоматичні процеси:
1. ✅ Checkout коду з GitHub
2. ✅ Налаштування Docker Buildx
3. ✅ Авторизація в Docker Hub
4. ✅ Генерація унікального тегу з Git commit hash
5. ✅ Збірка Docker образу
6. ✅ Push в Docker Hub (2 теги: commit hash + latest)
7. ✅ SSH підключення до AWS EC2
8. ✅ Встановлення Docker на сервері (якщо потрібно)
9. ✅ Зупинка старого контейнера
10. ✅ Завантаження нового образу
11. ✅ Запуск нового контейнера

## 🏗️ Архітектура
```
GitHub → GitHub Actions → Docker Hub → AWS EC2
  ↓           ↓              ↓            ↓
 Push      Build Image    Store        Deploy
```

## 📁 Структура проекту
```
lukianchuk-node-ci/
├── Dockerfile              # Інструкції для збірки образу
├── .dockerignore           # Виключення файлів з образу
├── index.js                # Node.js застосунок
├── package.json            # Залежності
├── .github/
│   └── workflows/
│       └── deploy.yml      # CI/CD pipeline
└── README.md
```

## 🐳 Docker Commands

### Локальна збірка
```bash
docker build -t yunona46/nodeapp:$(git rev-parse --short HEAD) .
```

### Запуск локально
```bash
docker run -d -p 3000:3000 --name nodeapp yunona46/nodeapp:latest
```

### Перевірка
```bash
curl http://localhost:3000
```

## 🌍 Деплой на AWS EC2

### Інфраструктура:
- **Регіон:** Europe (Stockholm)
- **Instance Type:** t2.micro (Free Tier)
- **OS:** Ubuntu Server 22.04 LTS
- **Порти:** 22 (SSH), 80 (HTTP), 443 (HTTPS), 3000 (Node.js)

### Доступ до застосунку:
```
http://ваша_EC2_IP:3000
```

## 🔐 GitHub Secrets

Налаштовані секрети:
- `DOCKERHUB_USERNAME` - Docker Hub username
- `DOCKERHUB_TOKEN` - Docker Hub access token
- `EC2_HOST` - AWS EC2 public IP
- `EC2_USER` - SSH username (ubuntu)
- `EC2_SSH_KEY` - Private SSH key (.pem)

## 🛠️ Технології

- **Node.js** (LTS) - Runtime
- **Docker** - Контейнеризація
- **GitHub Actions** - CI/CD
- **Docker Hub** - Registry
- **AWS EC2** - Hosting
- **Ubuntu 22.04** - OS

## 📦 Docker Hub

https://hub.docker.com/r/yunona46/nodeapp

## 👤 Автор

- GitHub: [@yunona46](https://github.com/yunona46)
- Docker Hub: [yunona46](https://hub.docker.com/u/yunona46)

---

**Практичне заняття 9** - Створення Dockerfile для застосунку Node.js
