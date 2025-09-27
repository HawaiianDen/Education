# Git — Уровень 4: Профессиональное использование (подробно)

## 14. Git Hooks

Git Hooks — это исполняемые скрипты, которые запускаются Git при определённых событиях (commit, push, merge, приём на сервере и т.д.). Они помогают автоматизировать проверки и внедрять правила.

### Типы хуков

**Client-side (локальные):**
- `pre-commit` — перед созданием коммита, можно запускать линтеры, запретить TODO/FIXME, проверить бинарные файлы.
- `commit-msg` — проверяет формат сообщения (например, наличие ID задачи).
- `pre-push` — быстрые тесты перед отправкой на сервер.
- Дополнительно: `prepare-commit-msg`, `post-commit`, `pre-rebase`, `post-merge`, `post-checkout`.

**Server-side (на сервере):**
- `pre-receive` — проверка push до приёма, можно отклонить.
- `update` — вызывается для каждого рефа.
- `post-receive` — триггеры для CI/CD, деплой, уведомления.

### Примеры

**pre-commit** — запрет TODO/FIXME:
```bash
#!/bin/sh
for f in $(git diff --cached --name-only); do
  grep -q "TODO\|FIXME" "$f" && { echo "TODO/FIXME found in $f"; exit 1; }
done
```

**commit-msg** — требование ID задачи:
```bash
#!/bin/sh
msg=$(cat "$1")
if ! echo "$msg" | grep -qE "[A-Z]{2,}-[0-9]+"; then
  echo "Commit must contain issue ID (e.g., ABC-123)"
  exit 1
fi
```

**pre-push** — быстрые тесты:
```bash
#!/bin/sh
if ! ./scripts/test.sh; then
  echo "Tests failed, push aborted"
  exit 1
fi
```

**pre-receive** — запрет прямого пуша в `main`:
```bash
#!/bin/sh
while read oldrev newrev ref; do
  [ "$ref" = "refs/heads/main" ] && exit 1
done
```

### Советы
- Держите хуки быстрыми.
- Критичное проверяйте на сервере.
- Для команды используйте `core.hooksPath` или инструменты (`pre-commit`, `husky`).

---

## 15. Подмодули (Submodules)

Подмодуль — это внешний репозиторий внутри проекта. Суперпроект фиксирует commit-id подмодуля.

### Добавление
```bash
git submodule add https://github.com/org/lib.git libs/lib
git commit -m "Add submodule"
```

### Клонирование с подмодулями
```bash
git clone --recurse-submodules url
```

Если без `--recurse-submodules`:
```bash
git submodule update --init --recursive
```

### Обновление
```bash
cd libs/lib
git pull origin main
cd ../..
git add libs/lib
git commit -m "Update submodule"
```

Или автоматически:
```bash
git submodule update --remote --merge
```

### Удаление
```bash
git submodule deinit -f libs/lib
git rm -f libs/lib
rm -rf .git/modules/libs/lib
```

### Плюсы и минусы
**Плюсы:** точный контроль версии зависимости.  
**Минусы:** detached HEAD, требует дополнительных коммитов.

**Альтернатива:** `git subtree`, пакетные менеджеры.

---

## 16. Поиск в истории

### `git grep` — поиск по коду
```bash
git grep -n "TODO"
git grep -n "init_db" origin/main
```

### `git log -S` и `-G`
- `-S<string>` ищет, где изменилось число вхождений строки.
- `-G<regex>` ищет по регулярке в diff.

```bash
git log -S"password" -- path/to/file
git log -G"fix_[0-9]+" -p -- file.c
```

### `git bisect` — поиск багов
```bash
git bisect start
git bisect bad
git bisect good v1.2.0
git bisect run ./test.sh
```

### `git blame` — автор строк
```bash
git blame -L 1,100 -e file.py
```

---

## 17. Оптимизация и производительность

### Очистка и сжатие
```bash
git gc --prune=now
git repack -ad
```

### Переписывание истории
```bash
git filter-branch --index-filter 'git rm -r --cached secret.txt' -- --all
```
Рекомендуется: `git filter-repo` или BFG (быстрее).

### Работа с большими файлами — Git LFS
```bash
git lfs install
git lfs track "*.bin"
git add .gitattributes
```

### Кэширование учетных данных
```bash
git config --global credential.helper cache   # временно
git config --global credential.helper store   # навсегда
```

