# Node.js CI/CD Project

## Опис
Простий Node.js застосунок з автоматичною збіркою Docker образів через GitHub Actions.

## Docker Commands

### Збірка образу локально
```bash
docker build -t yunona46/nodeapp:$(git rev-parse --short HEAD) .
```

### Запуск контейнера
```bash
docker run -d -p 3000:3000 --name nodeapp yunona46/nodeapp:latest
```

### Перевірка
```bash
curl http://localhost:3000
```

### Зупинка і видалення
```bash
docker stop nodeapp
docker rm nodeapp
```

## CI/CD Pipeline

При push в гілку main/master автоматично:
1. ✅ Збирається Docker образ
2. ✅ Образ позначається хешем коміту
3. ✅ Завантажується в Docker Hub

## Структура проекту
```
lukianchuk-node-ci/
├── Dockerfile           # Інструкції для збірки образу
├── .dockerignore        # Виключення файлів з образу
├── index.js             # Node.js застосунок
├── package.json         # Залежності проекту
├── .github/
│   └── workflows/
│       └── deploy.yml   # CI/CD pipeline
└── README.md
```

## Технології

- **Node.js** - Runtime середовище
- **Docker** - Контейнеризація
- **GitHub Actions** - CI/CD автоматизація
- **Docker Hub** - Реєстр образів

## Автор

yunona46
