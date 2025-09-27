# Уровень 4: DevOps-специфичные темы (Must Master)

---

## 21. Контейнеризация

### Docker основы
```bash
# Запуск контейнера
docker run -it ubuntu bash

# Список контейнеров
docker ps -a

# Список образов
docker images

# Остановка и удаление
docker stop <id>
docker rm <id>
docker rmi <image>
```

### Dockerfile
Пример минимального Dockerfile:
```dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```
```bash
docker build -t myapp:latest .
docker run -d -p 8080:8080 myapp:latest
```

### Docker Compose
Пример `docker-compose.yml`:
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - .:/code
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: example
```
```bash
docker-compose up -d
docker-compose down
```

### Оркестрация (базовые понятия)
- Kubernetes (k8s): pods, deployments, services.
- Swarm: встроенный в Docker кластер.

---

## 22. Конфигурационное управление

### Ansible
- Inventory (хосты): `inventory.ini`
```ini
[web]
web1 ansible_host=192.168.0.10
```

- Playbook:
```yaml
- hosts: web
  become: true
  tasks:
    - name: Установить nginx
      apt:
        name: nginx
        state: present
```
```bash
ansible-playbook -i inventory.ini site.yml
```

### Terraform (основы)
Пример конфигурации:
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```
```bash
terraform init
terraform plan
terraform apply
```

---

## 23. Системы контроля версий (Git)

```bash
git init
git clone url
git status
git add file
git commit -m "msg"
git push origin main
git pull origin main
git merge feature-branch
git rebase main
```

### Работа с GitHub/GitLab
- Pull Requests / Merge Requests.
- GitLab CI/CD integration.
- Branching strategies (GitFlow, trunk-based).

---

## 24. CI/CD концепции

### Основы
- **Pipeline** — набор шагов.
- **Stages** — сборка, тесты, деплой.
- **Artifacts** — результаты шагов.

### Jenkins (базово)
- Pipelines через `Jenkinsfile`:
```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps { sh 'make build' }
    }
    stage('Test') {
      steps { sh 'pytest' }
    }
  }
}
```

### GitLab CI (базово)
```yaml
stages:
  - build
  - test

build:
  stage: build
  script:
    - make build

test:
  stage: test
  script:
    - pytest
```

---

## 25. Логирование и мониторинг

### Centralized logging
- **ELK stack**: Elasticsearch + Logstash + Kibana.
- Filebeat для сбора логов.

### Monitoring
- **Prometheus** — сбор метрик.
- **Grafana** — визуализация.
- Метрики: CPU, память, запросы, latency.

---

## 26. Облачные платформы

### Основы AWS / Azure / GCP
- Compute: EC2 (AWS), VM (Azure), Compute Engine (GCP).
- Storage: S3 (AWS), Blob (Azure), Cloud Storage (GCP).
- Networking: VPC, Security Groups, Load Balancers.

### Общие концепции
- IAM (права доступа).
- Billing & cost optimization.
- Infrastructure as Code (Terraform, CloudFormation).

---
