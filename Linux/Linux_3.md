# Уровень 3: Будущий DevOps/Админ (Must Master)

---

## 14. Шеллы и скриптование (Bash)

### Основы Bash-скрипта
```bash
#!/usr/bin/env bash
set -euo pipefail      # безопасные опции
IFS=$'\n\t'

# переменные
NAME="world"
readonly CONST=1

# условие
if [[ -f "/etc/passwd" ]]; then
  echo "exists"
else
  echo "no"
fi

# цикл
for f in $(ls /etc); do
  echo $f
done

# функция
log(){ echo "[INFO] $*"; }
log "done"
```

### Аргументы и парсинг
```bash
while [[ $# -gt 0 ]]; do
  key="$1"; shift
  case $key in
    -f|--file) FILE="$1"; shift;;
    -v|--verbose) VERBOSE=1;;
    *) echo "Unknown $key"; exit 1;;
  esac
done
```

### Отладка
- `set -x` или `bash -x script.sh` — трассировка.
- `trap 'echo "ERR at $LINENO"; exit 1' ERR` — отладка ошибок.

### Best practices
- Используйте `set -euo pipefail`.
- Всегда заключайте переменные в кавычки.
- Делите код на функции.
- Логируйте ошибки в stderr.

---

## 15. Переменные окружения

### Где задавать
- Пользователь: `~/.bashrc`, `~/.profile`, `~/.bash_profile`.
- Системные: `/etc/profile`, `/etc/environment`, `/etc/profile.d/*.sh`.

### Команды
```bash
export PATH="$HOME/bin:$PATH"
export APP_ENV=production
env | grep APP
unset APP_ENV
```

**Различие:**
- `.bashrc` — интерактивные non-login шеллы.
- `.bash_profile`/`.profile` — login шеллы.

---

## 16. Планировщик задач

### Cron
Формат: `m h dom mon dow command`

Примеры:
```cron
30 2 * * * /usr/bin/backup.sh   # каждый день в 2:30
* * * * * /usr/bin/check.sh     # каждую минуту
```
```bash
crontab -e   # редактировать crontab
crontab -l   # список заданий
```

### Systemd timers
```ini
# myjob.timer
[Timer]
OnCalendar=daily
Persistent=true
```
```bash
systemctl enable --now myjob.timer
```

### at
```bash
echo "/path/script.sh" | at 23:00
```

---

## 17. Системные сервисы и демоны (systemd)

### Управление сервисами
```bash
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl status nginx
systemctl enable nginx   # автозапуск
systemctl disable nginx
```

### Логи
```bash
journalctl -u nginx --since "1 hour ago" -f
journalctl -xe
```

---

## 18. Мониторинг системы

### Что смотреть
- CPU load → `uptime`, `top`, `htop`.
- Memory → `free -h`, `vmstat`.
- Disk I/O → `iostat`, `iotop`.
- Network → `iftop`, `ss -tulnp`.

### Команды
```bash
top
htop
vmstat 1
iostat -xz 1
iotop -o
iftop -i eth0
```

---

## 19. Привилегии и sudo

### Основы
```bash
sudo usermod -aG sudo alice
sudo -i                 # root shell
su -                    # смена пользователя
visudo                  # редактирование sudoers
```

### Best practices
- Используйте sudo, не работайте от root.
- Не включайте `NOPASSWD` без нужды.
- Логируйте все действия root.

---

## 20. SSH и удалённое управление

### Ключи
```bash
ssh-keygen -t ed25519 -C "you@example.com"
ssh-copy-id user@host
```

### Подключение
```bash
ssh user@host
ssh -p 2222 user@host
ssh -A user@host
```

### SCP / RSYNC
```bash
scp file user@host:/tmp/
rsync -avz file user@host:/var/www/
```

### Tunneling / Forwarding
```bash
ssh -L 5432:localhost:5432 user@db     # local forward
ssh -R 9000:localhost:3000 user@host   # remote forward
ssh -D 1080 user@jump                  # dynamic socks proxy
```

### Безопасность
- `PasswordAuthentication no`
- `PermitRootLogin no`
- Используйте Fail2ban или UFW для защиты.

---
