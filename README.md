# Node.js CI/CD Project

## Docker Commands

### Build image
```bash
docker build -t lukianchuk/nodeapp:$(git rev-parse --short HEAD) .
```

### Run container
```bash
docker run -d -p 3000:3000 --name nodeapp lukianchuk/nodeapp:latest
```

### Stop and remove
```bash
docker stop nodeapp
docker rm nodeapp
```

## Deployment

The application automatically deploys to EC2 when pushing to main/master branch.

## Technologies

- Node.js
- Docker
- GitHub Actions
- AWS EC2
