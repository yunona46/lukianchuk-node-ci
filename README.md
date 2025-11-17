# Node.js CI/CD Project

Автор: **yunona46**

## 📋 Опис проекту

Простий Node.js застосунок з автоматичною збіркою Docker образів через GitHub Actions. При кожному push в main/master гілку автоматично збирається Docker образ та завантажується в Docker Hub з унікальним тегом на основі хеша Git коміту.

## 🚀 CI/CD Pipeline

### Автоматичні процеси:
1. ✅ Checkout коду з репозиторію
2. ✅ Налаштування Docker Buildx
3. ✅ Авторизація в Docker Hub
4. ✅ Генерація тегу з хеша Git коміту
5. ✅ Збірка Docker образу
6. ✅ Push образу в Docker Hub з двома тегами:
   - `yunona46/nodeapp:<git-hash>` - версія з хешем коміту
   - `yunona46/nodeapp:latest` - остання версія

## 📁 Структура проекту
lukianchuk-node-ci/
├── Dockerfile              # Інструкції для збірки Docker образу
├── .dockerignore           # Файли, які не копіюються в образ
├── index.js                # Основний файл Node.js застосунку
├── package.json            # Залежності та скрипти проекту
├── package-lock.json       # Заморожені версії залежностей
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Actions CI/CD workflow
└── README.md               # Документація проекту

## 🐳 Docker Commands

### Локальна збірка образу
```bash
# Збірка з тегом на основі Git хеша
docker build -t yunona46/nodeapp:$(git rev-parse --short HEAD) .

# Збірка з тегом latest
docker build -t yunona46/nodeapp:latest .
```

### Завантаження образу з Docker Hub
```bash
docker pull yunona46/nodeapp:latest
```

### Запуск контейнера
```bash
# Запуск у фоновому режимі на порту 3000
docker run -d -p 3000:3000 --name nodeapp yunona46/nodeapp:latest

# Запуск з автоматичним перезапуском
docker run -d -p 3000:3000 --name nodeapp --restart unless-stopped yunona46/nodeapp:latest
```

### Перевірка роботи
```bash
# Через curl
curl http://localhost:3000

# Або відкрийте в браузері
# http://localhost:3000
```

### Перегляд логів
```bash
docker logs nodeapp
docker logs -f nodeapp  # з постійним оновленням
```

### Управління контейнером
```bash
# Зупинка контейнера
docker stop nodeapp

# Запуск зупиненого контейнера
docker start nodeapp

# Перезапуск контейнера
docker restart nodeapp

# Видалення контейнера
docker rm nodeapp

# Видалення контейнера (з примусовою зупинкою)
docker rm -f nodeapp
```

### Робота з образами
```bash
# Перегляд локальних образів
docker images yunona46/nodeapp

# Видалення образу
docker rmi yunona46/nodeapp:latest

# Очищення невикористовуваних образів
docker image prune -a
```

## 🔧 Dockerfile

Образ базується на офіційному `node:lts` та включає:
- Встановлення залежностей через npm
- Кешування шарів для швидшої збірки
- Відкриття порту 3000
- Команду запуску Node.js застосунку

## 📝 .dockerignore

Виключає з образу:
- `node_modules` (встановлюються в контейнері)
- `.env` (секретні дані)
- `.git` (історія Git)
- Інші непотрібні файли

## 🔐 GitHub Secrets

Для роботи CI/CD налаштовані:
- `DOCKERHUB_USERNAME` - ім'я користувача Docker Hub
- `DOCKERHUB_TOKEN` - Personal Access Token для авторизації

## 🛠️ Технології

- **Node.js** (LTS) - JavaScript runtime
- **Docker** - Платформа контейнеризації
- **GitHub Actions** - CI/CD автоматизація
- **Docker Hub** - Реєстр Docker образів

## 📦 Docker Hub

Образи доступні за адресою:
https://hub.docker.com/r/yunona46/nodeapp

## 👤 Автор

GitHub: [@yunona46](https://github.com/yunona46)
Docker Hub: [yunona46](https://hub.docker.com/u/yunona46)

## 📄 Ліцензія

Цей проект створено для навчальних цілей.
