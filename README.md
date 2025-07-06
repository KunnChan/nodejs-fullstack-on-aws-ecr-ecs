### This project is the demo project used in this video: https://youtu.be/P91vW4fnQHI

> Use docker-compose.yml to test in local

# EC2

> This is to connect to RDS

```bash
sudo yum update -y
sudo dnf install -y mariadb105
```

```sql
CREATE DATABASE IF NOT EXISTS crud_app;
USE crud_app;

CREATE TABLE users (
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(100),
email VARCHAR(100)
);
```

By default, Docker Desktop on Mac (M1/M2) or Apple Silicon builds images for linux/arm64.
But ECS Fargate only supports linux/amd64 currently.

```bash
# for other chip
docker build -t node-crud-app .

# for Apple Silicon chip
docker buildx build --platform linux/amd64 --load --tag node-crud-app .

```

### Create ECR Repo

```bash
aws ecr create-repository --repository-name node-crud-app
```

### Login to ECR

```bash
aws ecr get-login-password | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
```

### Tag and Push image to ECR

```bash
docker tag node-crud-app:latest <account-id>.dkr.ecr.<region>.amazonaws.com/node-crud-app

docker push <account-id>.dkr.ecr.<region>.amazonaws.com/node-crud-app

```
