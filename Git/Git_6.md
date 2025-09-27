# Git — Уровень 6: Для DevOps/Администрирования

## 21. Управление Git серверами

### GitLab self-hosted
- Установка через Omnibus-пакет или Docker.
- Настройка домена и HTTPS (через Let’s Encrypt).
- Конфигурация ресурсов (Redis, PostgreSQL, Sidekiq).

### Управление репозиториями и пользователями
- Создание групп и проектов.
- Роли доступа: Guest, Reporter, Developer, Maintainer, Owner.
- Шаблоны проектов для единых стандартов.

### Backup и восстановление
```bash
# Создать бэкап
gitlab-backup create
# Восстановить
gitlab-backup restore BACKUP=timestamp
```
- Регулярные бэкапы базы и файлов.
- Хранение копий вне основной инфраструктуры.

### Мониторинг и логирование
- Встроенные метрики Prometheus.
- Логи GitLab: `/var/log/gitlab/`.
- Интеграция с ELK/Graylog для централизации.

---

## 22. Безопасность Git

### SSH ключи
- Использовать `ed25519` или `rsa-4096`.
- Удалять старые ключи.

### Подписанные коммиты (GPG)
```bash
gpg --full-generate-key
git config --global user.signingkey <KEYID>
git commit -S -m "Signed commit"
```

### Сканирование секретов
- Инструменты: `git-secrets`, `trufflehog`, GitGuardian.
- Настройка pre-commit hook для блокировки утечек.

### Security policies
- Обязательное использование MR/PR.
- Защищённые ветки (`main`, `release/*`).
- Code Owners для критичных файлов.

---

## 23. Автоматизация с Git

### Git hooks для DevOps
- `post-receive` — запуск деплоя.
- `pre-receive` — проверка политики (например, запрет пуша в main).

### Интеграция с развертыванием
- Автозапуск Ansible/Puppet/Chef скриптов после merge.
- Использование GitOps (ArgoCD, Flux).

### Автоматическое версионирование
- Semantic Versioning + теги.
- Инструменты: `semantic-release`, `bump2version`.

### Git в IaC (Infrastructure as Code)
- Хранение Terraform/Helm чарты в Git.
- Автоматический деплой при merge.

---

## 24. Мониторинг и аналитика

### Метрики Git
- Кол-во коммитов, PR/MR, время ревью.
- Лидеры по вкладу.

### Анализ активности команды
- GitLab Insights, GitHub Insights.
- Визуализация через Grafana.

### Bottlenecks
- Долгое ревью.
- Длительные CI/CD пайплайны.
- Большие Pull Requests.

### Интеграция с мониторингом
- Отправка метрик в Prometheus.
- Алерты по SLA (например, время merge > 2 дня).

---

## Итог
Уровень 6 — это **администрирование и DevOps**: управление Git-серверами, безопасность, автоматизация процессов и аналитика для повышения эффективности команды.
