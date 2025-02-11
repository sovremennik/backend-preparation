### **Docker для Backend Developer**

---

#### 1. **Основы Docker**
- **Что такое Docker:**  
  - Платформа для контейнеризации приложений.  
  - Изоляция приложений и зависимостей.  
- **Преимущества Docker:**  
  - Повторяемость окружений.  
  - Легкость в развертывании.  
  - Экономия ресурсов.  
- **Ключевые компоненты:**  
  - Docker Engine.  
  - Docker CLI.  
  - Docker Hub.  

---

#### 2. **Установка и настройка**
- **Установка Docker:**  
  - Установка на Windows, macOS, Linux.  
- **Базовая настройка:**  
  - Проверка установки (`docker --version`).  
  - Настройка прав доступа.  

---

#### 3. **Образы (Images)**
- **Что такое Docker Image:**  
  - Шаблон для создания контейнеров.  
- **Работа с образами:**  
  - `docker pull <image>` — загрузка образа.  
  - `docker images` — список локальных образов.  
  - `docker rmi <image>` — удаление образа.  
- **Создание собственных образов:**  
  - Написание `Dockerfile`.  
  - Команда `docker build`.  

---

#### 4. **Контейнеры**
- **Что такое Docker Container:**  
  - Экземпляр образа.  
- **Основные команды:**  
  - `docker run <image>` — запуск контейнера.  
  - `docker ps` — просмотр активных контейнеров.  
  - `docker stop <container>` — остановка контейнера.  
  - `docker rm <container>` — удаление контейнера.  
- **Параметры запуска:**  
  - Флаги `-d`, `-p`, `-v`.  
  - Переменные окружения (`-e`).  

---

#### 5. **Работа с Dockerfile**
- **Что такое Dockerfile:**  
  - Файл инструкций для сборки образа.  
- **Ключевые инструкции:**  
  - `FROM` — базовый образ.  
  - `COPY` и `ADD` — добавление файлов.  
  - `RUN` — выполнение команд.  
  - `CMD` и `ENTRYPOINT` — команды запуска контейнера.  
  - `ENV` — переменные окружения.  
  - `EXPOSE` — открытие портов.  
- **Сборка образа:**  
  - `docker build -t <image-name> .`  

---

#### 6. **Сети (Networking)**
- **Типы сетей Docker:**  
  - Bridge, Host, None.  
- **Работа с сетями:**  
  - `docker network ls` — список сетей.  
  - `docker network create <name>` — создание сети.  
  - `docker network connect` — подключение контейнера к сети.  

---

#### 7. **Томá (Volumes)**
- **Что такое Volume:**  
  - Способ хранения данных контейнеров.  
- **Работа с томами:**  
  - `docker volume create <name>` — создание тома.  
  - `docker run -v <volume>:/path` — монтирование тома.  
  - `docker volume ls` — список томов.  
  - `docker volume rm <name>` — удаление тома.  

---

#### 8. **Docker Compose**
- **Что такое Docker Compose:**  
  - Инструмент для управления несколькими контейнерами.  
- **Файл `docker-compose.yml`:**  
  - Определение сервисов.  
  - Настройка сети и томов.  
  - Пример:  
    ```yaml
    version: '3.8'
    services:
      app:
        image: my-app
        ports:
          - "8080:8080"
        volumes:
          - app-data:/data
    volumes:
      app-data:
    ```
- **Команды Docker Compose:**  
  - `docker-compose up` — запуск всех сервисов.  
  - `docker-compose down` — остановка и удаление контейнеров.  

---

#### 9. **Лучшие практики**
- **Оптимизация Dockerfile:**  
  - Минимизация количества слоев.  
  - Использование легковесных базовых образов (например, `alpine`).  
- **Работа с секретами:**  
  - Использование `docker secrets`.  
  - Избегать хранения секретов в Dockerfile.  
- **Логирование и мониторинг:**  
  - Настройка логов через драйверы.  
  - Интеграция с инструментами (ELK Stack, Prometheus).  

---

#### 10. **Docker в CI/CD**
- **Роль Docker в CI/CD:**  
  - Сборка, тестирование, деплой через контейнеры.  
- **Интеграция с Jenkins/GitHub Actions/GitLab CI:**  
  - Использование Docker агента.  
  - Примеры пайплайнов.  

---

#### 11. **Безопасность**
- **Минимизация уязвимостей:**  
  - Регулярное обновление образов.  
  - Использование проверенных базовых образов.  
- **Проверка образов на уязвимости:**  
  - Использование инструментов, например, `trivy`.  
- **Ограничение ресурсов контейнеров:**  
  - `--memory`, `--cpu-shares`.  
- **Запуск без root-привилегий:**  
  - Указание `USER` в Dockerfile.  

---

#### 12. **Траблшутинг и отладка**
- **Просмотр логов контейнера:**  
  - `docker logs <container>`.  
- **Выполнение команд внутри контейнера:**  
  - `docker exec -it <container> /bin/bash`.  
- **Анализ состояния контейнера:**  
  - `docker inspect <container>`.  
  - `docker stats`.  

---

#### 13. **Интеграция с Kubernetes**
- **Docker и Kubernetes:**  
  - Использование Docker-образов для деплоя в Kubernetes.  
- **Пример деплоя:**  
  - Создание Pod с Docker-образом.  

---

#### 14. **Полезные команды и инструменты**
- **Команды:**  
  - `docker prune` — удаление неиспользуемых данных.  
  - `docker history <image>` — история образа.  
- **Инструменты:**  
  - Portainer — GUI для управления контейнерами.  
  - Docker Desktop — управление через графический интерфейс.  

---

📁 docker/
├── 📄 docker.md
├── 📁 1-basics/
│   ├── 📄 1-basics.md
│   ├── 📁 core-concepts/
│   │   └── 📄 core-concepts.md
│   └── 📁 advantages/
│       └── 📄 advantages.md
├── 📁 2-installation/
│   ├── 📄 2-installation.md
│   ├── 📁 setup/
│   │   └── 📄 setup.md
│   └── 📁 configuration/
│       └── 📄 configuration.md
├── 📁 3-images/
│   ├── 📄 3-images.md
│   ├── 📁 working-with-images/
│   │   └── 📄 working-with-images.md
│   └── 📁 creating-images/
│       └── 📄 creating-images.md
├── 📁 4-containers/
│   ├── 📄 4-containers.md
│   ├── 📁 container-commands/
│   │   └── 📄 container-commands.md
│   └── 📁 container-options/
│       └── 📄 container-options.md
├── 📁 5-dockerfile/
│   ├── 📄 5-dockerfile.md
│   ├── 📁 dockerfile-instructions/
│   │   └── 📄 dockerfile-instructions.md
│   └── 📁 building-images/
│       └── 📄 building-images.md
├── 📁 6-networking/
│   ├── 📄 6-networking.md
│   ├── 📁 network-types/
│   │   └── 📄 network-types.md
│   └── 📁 working-with-networks/
│       └── 📄 working-with-networks.md
├── 📁 7-volumes/
│   ├── 📄 7-volumes.md
│   ├── 📁 volume-basics/
│   │   └── 📄 volume-basics.md
│   └── 📁 working-with-volumes/
│       └── 📄 working-with-volumes.md
├── 📁 8-docker-compose/
│   ├── 📄 8-docker-compose.md
│   ├── 📁 compose-basics/
│   │   └── 📄 compose-basics.md
│   └── 📁 compose-commands/
│       └── 📄 compose-commands.md
├── 📁 9-best-practices/
│   ├── 📄 9-best-practices.md
│   ├── 📁 optimizing-dockerfile/
│   │   └── 📄 optimizing-dockerfile.md
│   └── 📁 secrets-management/
│       └── 📄 secrets-management.md
├── 📁 10-ci-cd/
│   ├── 📄 10-ci-cd.md
│   ├── 📁 ci-cd-basics/
│   │   └── 📄 ci-cd-basics.md
│   └── 📁 integration-with-tools/
│       └── 📄 integration-with-tools.md
├── 📁 11-security/
│   ├── 📄 11-security.md
│   ├── 📁 vulnerability-management/
│   │   └── 📄 vulnerability-management.md
│   └── 📁 resource-limits/
│       └── 📄 resource-limits.md
├── 📁 12-troubleshooting/
│   ├── 📄 12-troubleshooting.md
│   ├── 📁 logging/
│   │   └── 📄 logging.md
│   └── 📁 debugging/
│       └── 📄 debugging.md
├── 📁 13-kubernetes/
│   ├── 📄 13-kubernetes.md
│   ├── 📁 docker-and-kubernetes/
│   │   └── 📄 docker-and-kubernetes.md
│   └── 📁 deployment-examples/
│       └── 📄 deployment-examples.md
└── 📁 14-tools-and-commands/
  ├── 📄 14-tools-and-commands.md
  ├── 📁 useful-commands/
  │   └── 📄 useful-commands.md
  └── 📁 docker-tools/
        └── 📄 docker-tools.md