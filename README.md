# Node.js CI/CD Project with Docker Compose & PostgreSQL

Автор: **yunona46**

## 📋 Опис

Повноцінний Node.js застосунок з PostgreSQL базою даних, автоматичною CI/CD через GitHub Actions та деплоєм на AWS EC2 через Docker Compose.

## 🏗️ Архітектура
```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│   GitHub    │────▶│GitHub Actions│────▶│  Docker Hub │
└─────────────┘     └──────────────┘     └─────────────┘
                           │                      │
                           ▼                      ▼
                    ┌──────────────┐       ┌─────────────┐
                    │   AWS EC2    │◀──────│Docker Compose│
                    └──────────────┘       └─────────────┘
                           │
                    ┌──────┴──────┐
                    ▼             ▼
            ┌─────────────┐  ┌─────────────┐
            │  Node.js    │  │ PostgreSQL  │
            │  Container  │──│  Container  │
            └─────────────┘  └─────────────┘
```

## 🚀 Функціонал

### API Endpoints:

- **GET /** - Головна сторінка з лічильником візитів
- **GET /db-check** - Перевірка підключення до БД
- **GET /stats** - Статистика візитів
- **GET /health** - Health check

### База даних:

- **PostgreSQL 15** (Alpine)
- Таблиця isits для збереження статистики
- Автоматичне створення схеми
- Persistent volume для збереження даних

## 📁 Структура проекту
```
lukianchuk-node-ci/
├── Dockerfile              # Multi-stage build для Node.js
├── docker-compose.yml      # Оркестрація сервісів
├── .dockerignore           # Виключення файлів
├── index.js                # Express + PostgreSQL
├── package.json            # Залежності (express, pg)
├── .github/
│   └── workflows/
│       └── deploy.yml      # CI/CD з docker-compose
└── README.md
```

## 🐳 Docker Compose

### Сервіси:

**app** (Node.js):
- Image: yunona46/nodeapp:latest
- Port: 3000
- Змінні: DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASSWORD
- Залежить від: db (з health check)

**db** (PostgreSQL):
- Image: postgres:15-alpine
- Port: 5432
- Volume: postgres_data (persistent storage)
- Health check: pg_isready

### Запуск локально:
```bash
# Збірка і запуск
docker-compose up --build

# У фоні
docker-compose up -d

# Перегляд логів
docker-compose logs -f

# Зупинка
docker-compose down

# Зупинка з видаленням volumes
docker-compose down -v
```

## 🔧 Локальна розробка
```bash
# Встановлення залежностей
npm install

# Запуск PostgreSQL локально (через Docker)
docker run -d \
  --name postgres-local \
  -e POSTGRES_DB=mydb \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres123 \
  -p 5432:5432 \
  postgres:15-alpine

# Запуск Node.js застосунку
DB_HOST=localhost npm start

# Тестування
curl http://localhost:3000
curl http://localhost:3000/db-check
curl http://localhost:3000/stats
```

## 📊 CI/CD Pipeline

### Етапи:

1. ✅ Checkout коду
2. ✅ Setup Docker Buildx
3. ✅ Login до Docker Hub
4. ✅ Генерація Git commit hash тегу
5. ✅ Build та push Docker образу
6. ✅ SSH підключення до EC2
7. ✅ Встановлення Docker (якщо потрібно)
8. ✅ Створення docker-compose.yml на сервері
9. ✅ Pull нового образу
10. ✅ Запуск через docker-compose

### Автоматичний деплой:

- ⚡ Trigger: Push в main/master
- 🎯 Target: AWS EC2 (Europe Stockholm)
- 📦 Method: Docker Compose
- ⏱️ Duration: ~2-3 хвилини

## 🌍 Deployment

### AWS EC2 Infrastructure:

- **Region:** Europe (Stockholm)
- **Instance:** t2.micro (Free Tier)
- **OS:** Ubuntu 22.04 LTS
- **Ports:** 22 (SSH), 80, 443, 3000 (App), 5432 (PostgreSQL)

### Доступ:
```
🌐 Application: http://<EC2_IP>:3000
📊 DB Check:    http://<EC2_IP>:3000/db-check
📈 Statistics:  http://<EC2_IP>:3000/stats
🏥 Health:      http://<EC2_IP>:3000/health
```

## 🔐 GitHub Secrets

Налаштовані секрети (5):

- **DOCKERHUB_USERNAME** - Docker Hub username
- **DOCKERHUB_TOKEN** - Personal Access Token
- **EC2_HOST** - AWS EC2 Public IPv4
- **EC2_USER** - SSH username (ubuntu)
- **EC2_SSH_KEY** - Private SSH key (.pem)

## 🛠️ Технології

### Backend:
- **Node.js** (LTS) - JavaScript runtime
- **Express** - Web framework
- **pg** - PostgreSQL client

### Database:
- **PostgreSQL 15** - Relational database
- **Alpine Linux** - Lightweight image

### DevOps:
- **Docker** - Контейнеризація
- **Docker Compose** - Оркестрація
- **GitHub Actions** - CI/CD
- **Docker Hub** - Registry
- **AWS EC2** - Cloud hosting

## 📦 Docker Hub

https://hub.docker.com/r/yunona46/nodeapp

### Tags:
- latest - Остання версія
- <git-hash> - Версія з commit hash

## 🧪 Тестування
```bash
# Health check
curl http://<EC2_IP>:3000/health

# Database connection
curl http://<EC2_IP>:3000/db-check

# Statistics
curl http://<EC2_IP>:3000/stats

# Main page (відкрийте у браузері)
http://<EC2_IP>:3000
```

## 📝 SSH Management
```bash
# Підключення до EC2
ssh -i nodejs-app-key-2025.pem ubuntu@<EC2_IP>

# Перегляд контейнерів
docker ps

# Логи застосунку
docker logs nodeapp

# Логи PostgreSQL
docker logs postgres-db

# Docker Compose статус
cd ~/nodeapp
docker-compose ps

# Перезапуск сервісів
docker-compose restart
```

## 👤 Автор

- **GitHub:** [@yunona46](https://github.com/yunona46)
- **Docker Hub:** [yunona46](https://hub.docker.com/u/yunona46)
- **Repository:** [lukianchuk-node-ci](https://github.com/yunona46/lukianchuk-node-ci)

## 📄 Ліцензія

Навчальний проект для курсу DevOps.

---

**Практичне заняття 9** - Створення Dockerfile + Docker Compose для Node.js застосунку
