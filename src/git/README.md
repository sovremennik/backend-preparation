### **Git для Backend Developer**

---

#### 1. **Введение в Git**
- **Что такое Git:**  
  - Система контроля версий для отслеживания изменений в коде.  
- **Зачем использовать Git:**  
  - Управление версиями.  
  - Совместная работа над проектом.  
  - История изменений и возможность отката.  
- **Основные концепты:**  
  - Репозиторий (локальный и удаленный).  
  - Коммиты.  
  - Ветвление.  

---

#### 2. **Основные команды**
- **Инициализация и настройка:**  
  - `git init` — создание репозитория.  
  - `git config` — настройка имени и почты.  
- **Работа с изменениями:**  
  - `git status` — состояние файлов.  
  - `git add` — добавление файлов в индекс.  
  - `git commit` — фиксация изменений.  
  - `git log` — просмотр истории изменений.  
- **Отмена изменений:**  
  - `git restore` — отмена изменений в файлах.  
  - `git reset` — сброс изменений.  

---

#### 3. **Ветвление и слияние**
- **Работа с ветками:**  
  - `git branch` — просмотр списка веток.  
  - `git branch <branch-name>` — создание новой ветки.  
  - `git checkout <branch-name>` — переключение на ветку.  
  - `git switch` — современная альтернатива `checkout`.  
- **Слияние веток:**  
  - `git merge` — объединение веток.  
  - Разрешение конфликтов.  
- **Удаление веток:**  
  - `git branch -d <branch-name>` — удаление локальной ветки.  

---

#### 4. **Работа с удаленными репозиториями**
- **Подключение репозитория:**  
  - `git remote add origin <url>` — добавление удаленного репозитория.  
- **Команды для работы с удаленным репозиторием:**  
  - `git push` — отправка изменений.  
  - `git pull` — получение изменений.  
  - `git fetch` — загрузка данных без слияния.  
- **Клонирование репозитория:**  
  - `git clone <url>` — копирование удаленного репозитория.  

---

#### 5. **Ревью и совместная работа**
- **Pull Requests (PR):**  
  - Что такое PR.  
  - Как создавать PR (GitHub, GitLab).  
  - Code Review: лучшие практики.  
- **Работа с Fork:**  
  - `git remote add upstream <url>` — синхронизация с оригинальным репозиторием.  
  - `git rebase` — подтягивание изменений из оригинального репозитория.  

---

#### 6. **Rebase и Squash**
- **`git rebase`:**  
  - Перенос коммитов из одной ветки в другую.  
  - Использование `--interactive` для изменения истории.  
- **Squash:**  
  - Объединение нескольких коммитов в один.  
  - `git rebase -i HEAD~<n>` — выбор коммитов для объединения.  

---

#### 7. **Stash и временное сохранение**
- **Сохранение изменений без коммита:**  
  - `git stash` — сохранение текущих изменений.  
  - `git stash apply` — восстановление изменений.  
  - `git stash drop` — удаление сохраненных изменений.  
- **Полезные сценарии:**  
  - Переключение веток без потери текущих изменений.  

---

#### 8. **Просмотр и сравнение изменений**
- **Просмотр изменений:**  
  - `git diff` — сравнение файлов.  
  - `git show` — просмотр конкретного коммита.  
- **Сравнение веток:**  
  - `git diff <branch1>..<branch2>`.  

---

#### 9. **Git Hooks**
- **Что такое Git Hooks:**  
  - Сценарии, выполняемые при определенных событиях (коммит, пуш и т.д.).  
- **Примеры использования:**  
  - Автоматическая проверка кода перед коммитом.  
  - Запуск тестов перед пушем.  

---

#### 10. **Работа с тегами**
- **Создание тегов:**  
  - `git tag <tag-name>` — создание аннотированного тега.  
  - `git tag -a <tag-name> -m "<message>"`.  
- **Отправка тегов:**  
  - `git push origin <tag-name>`.  
  - `git push origin --tags` — отправка всех тегов.  

---

#### 11. **Обработка конфликтов**
- **Что такое конфликт:**  
  - Причины возникновения.  
  - Как их обнаружить (сообщение Git).  
- **Разрешение конфликтов:**  
  - Ручное исправление.  
  - Использование инструментов (например, в IDE).  

---

#### 12. **Отладка и восстановление**
- **Отмена изменений:**  
  - `git revert <commit>` — отмена конкретного коммита.  
  - `git reset <commit>` — сброс изменений до определенного коммита.  
- **Восстановление файлов:**  
  - `git checkout -- <file>` для возврата к последнему коммиту.  

---

#### 13. **Git Flow и другие стратегии ветвления**
- **Что такое Git Flow:**  
  - Основные ветки: `main`, `develop`, `feature`, `release`, `hotfix`.  
  - Инструменты: `git-flow` CLI.  
- **Альтернативные подходы:**  
  - Trunk-based development.  
  - GitHub Flow.  

---

#### 14. **CI/CD и Git**
- **Интеграция с CI/CD:**  
  - Автоматический запуск тестов при пуше.  
  - Деплой на основе веток или тегов.  
- **Практики:**  
  - Использование `.gitignore` для исключения файлов из репозитория.  
  - Настройка protected-веток.  

---

#### 15. **Полезные инструменты**
- **Git GUI:**  
  - Sourcetree, GitKraken, IntelliJ IDEA встроенные возможности.  
- **Расширения Git:**  
  - `git blame` — определение автора изменений.  
  - `git bisect` — поиск коммита, вызвавшего баг.  
  - `git cherry-pick` — выборочный перенос коммитов.  

---
📁 git/
├── 📄 git.md
├── 📁 1-introduction/
│   ├── 📄 1-introduction.md
│   ├── 📁 what-is-git/
│   │   └── 📄 what-is-git.md
│   └── 📁 basic-concepts/
│       └── 📄 basic-concepts.md
├── 📁 2-basic-commands/
│   ├── 📄 2-basic-commands.md
│   ├── 📁 initialization/
│   │   └── 📄 initialization.md
│   └── 📁 working-with-changes/
│       └── 📄 working-with-changes.md
├── 📁 3-branching-and-merging/
│   ├── 📄 3-branching-and-merging.md
│   ├── 📁 working-with-branches/
│   │   └── 📄 working-with-branches.md
│   └── 📁 merging/
│       └── 📄 merging.md
├── 📁 4-remote-repositories/
│   ├── 📄 4-remote-repositories.md
│   ├── 📁 connecting-repositories/
│   │   └── 📄 connecting-repositories.md
│   └── 📁 remote-commands/
│       └── 📄 remote-commands.md
├── 📁 5-code-review/
│   ├── 📄 5-code-review.md
│   ├── 📁 pull-requests/
│   │   └── 📄 pull-requests.md
│   └── 📁 working-with-forks/
│       └── 📄 working-with-forks.md
├── 📁 6-rebase-and-squash/
│   ├── 📄 6-rebase-and-squash.md
│   ├── 📁 rebase/
│   │   └── 📄 rebase.md
│   └── 📁 squash/
│       └── 📄 squash.md
├── 📁 7-stash/
│   ├── 📄 7-stash.md
│   ├── 📁 saving-changes/
│   │   └── 📄 saving-changes.md
│   └── 📁 useful-scenarios/
│       └── 📄 useful-scenarios.md
├── 📁 8-viewing-changes/
│   ├── 📄 8-viewing-changes.md
│   ├── 📁 git-diff/
│   │   └── 📄 git-diff.md
│   └── 📁 comparing-branches/
│       └── 📄 comparing-branches.md
├── 📁 9-git-hooks/
│   ├── 📄 9-git-hooks.md
│   ├── 📁 introduction/
│   │   └── 📄 introduction.md
│   └── 📁 examples/
│       └── 📄 examples.md
├── 📁 10-tags/
│   ├── 📄 10-tags.md
│   ├── 📁 creating-tags/
│   │   └── 📄 creating-tags.md
│   └── 📁 pushing-tags/
│       └── 📄 pushing-tags.md
├── 📁 11-conflict-resolution/
│   ├── 📄 11-conflict-resolution.md
│   ├── 📁 understanding-conflicts/
│   │   └── 📄 understanding-conflicts.md
│   └── 📁 resolving-conflicts/
│       └── 📄 resolving-conflicts.md
├── 📁 12-debugging-and-recovery/
│   ├── 📄 12-debugging-and-recovery.md
│   ├── 📁 undoing-changes/
│   │   └── 📄 undoing-changes.md
│   └── 📁 file-recovery/
│       └── 📄 file-recovery.md
├── 📁 13-branching-strategies/
│   ├── 📄 13-branching-strategies.md
│   ├── 📁 git-flow/
│   │   └── 📄 git-flow.md
│   └── 📁 alternative-approaches/
│       └── 📄 alternative-approaches.md
├── 📁 14-ci-cd-and-git/
│   ├── 📄 14-ci-cd-and-git.md
│   ├── 📁 ci-cd-integration/
│   │   └── 📄 ci-cd-integration.md
│   └── 📁 best-practices/
│       └── 📄 best-practices.md
└── 📁 15-useful-tools/
  ├── 📄 15-useful-tools.md
  ├── 📁 git-gui/
  │   └── 📄 git-gui.md
  └── 📁 git-extensions/
  └── 📄 git-extensions.md