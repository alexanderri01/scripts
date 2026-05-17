# scripts
# 🚀 Universal DevOps & Monitoring Stack Installer

[English](#english) | [Русский](#русский)

---

## English

An interactive Python script designed to automatically provision and deploy up to 10 popular monitoring, logging, and alerting tools. Fully optimized for **Red Hat Enterprise Linux 10 (RHEL 10)** using rootless **Podman** and **Ubuntu** using native **Docker**.

### ✨ Features
* **Smart Numbered Menu:** No more manual name typing. Select components via a clean numbered list (e.g., `1, 2, 5`).
* **Robust Input Parsing:** Powered by regular expressions (`re`). Bulletproof against extra spaces, typos, or accidental terminal copy-paste anomalies.
* **Intelligent OS Detection:** Automatically configures rootless environments, registries, and system sockets on RHEL 10, or auto-installs Docker and Docker Compose on clean Ubuntu environments.
* **SELinux & Permissions Fixes:** Pre-configured security options (`label=disable`, proper host volumes) for tools like `process-exporter` and `cAdvisor` to read host statistics safely without tripping SELinux policies.

### 📋 Supported Components
| # | Tool | Purpose | Default Port |
| :-: | :--- | :--- | :-: |
| **1** | **Prometheus** | Core Time-Series Metric Database | `9090` |
| **2** | **Grafana** | Dashboard Visualization Engine | `3000` |
| **3** | **Node Exporter** | Host OS & Hardware Metrics | `9100` |
| **4** | **Process Exporter**| Granular Process Tracking Engine | `9256` |
| **5** | **Elasticsearch** | Powerful Log Database (ELK Stack) | `9200` |
| **6** | **Kibana** | Log Exploration & Visualization (ELK Stack) | `5601` |
| **7** | **Logstash** | Log Processing Pipeline (ELK Stack) | `5044` / `9600` |
| **8** | **Filebeat** | Lightweight Log Shipper | Local agent |
| **9** | **cAdvisor** | Container Resource & Usage Analyzer | `8080` |
| **10**| **Alertmanager** | Alert Routing & Notification Handler | `9093` |

### 🛠 Quick Start

Run the installer on your remote server with a single command:

```bash
curl -O [https://raw.githubusercontent.com/alexanderri01/scripts/main/deploy.py]
(https://raw.githubusercontent.com/alexanderri01/rhel10-monitoring-stack/main/deploy.py) && chmod +x deploy.py && ./deploy.py

#,Инструмент,Назначение,Порт по умолчанию
1,Prometheus,Основная база данных временных рядов (Метрики),9090
2,Grafana,Визуализация метрик и аналитические панели,3000
3,Node Exporter,Сбор метрик операционной системы и железа хоста,9100
4,Process Exporter,Детальный мониторинг запущенных процессов,9256
5,Elasticsearch,Высокопроизводительная база данных логов (ELK),9200
6,Kibana,"Поиск, анализ и визуализация логов (ELK)",5601
7,Logstash,"Конвейер для приема, фильтрации и отправки логов (ELK)",5044 / 9600
8,Filebeat,Легковесный агент для отправки системных логов,Локальный
9,cAdvisor,Анализ производительности и ресурсов контейнеров,8080
10,Alertmanager,Обработка и маршрутизация алертов из Prometheus,9093

🛠 Быстрый запуск
Запустите установщик на удаленном сервере одной командой:

Bash
curl -O [https://raw.githubusercontent.com/alexanderri01/scripts/main/deploy.py]
(https://raw.githubusercontent.com/alexanderri01/rhel10-monitoring-stack/main/deploy.py) && chmod +x deploy.py && ./deploy.py
📌 Примечание для пользователей Ubuntu: Если Docker устанавливается скриптом впервые, текущая сессия автоматически выполнит запуск через sudo, чтобы обойти ограничение прав сокета без необходимости перезаходить на сервер. Все последующие запуски будут работать без sudo.
===========================================================================================================================================

Вот обновленные секции для вашего `README.md`, описывающие, как масштабировать скрипт и добавлять новые контейнеры. Вы можете вставить этот блок в самый конец вашего текущего файла.

---

```markdown
---

## 🛠 How to Add New Containers / Как добавлять новые контейнеры

### English

The `deploy.py` script is built declaratively. If you want to expand the list of available tools in the future (e.g., adding MySQL, Redis, or Nginx), you only need to modify **two places** in the code:

#### Step 1: Update the Selection Menu (`TOOLS_MAP`)
Find the `TOOLS_MAP` dictionary at the top of the script and add your new item with the next incremental ID:
```python
TOOLS_MAP = {
    1: ("Prometheus", "prometheus"),
    # ...
    10: ("Alertmanager (Менеджер алертов)", "alertmanager"),
    11: ("PostgreSQL (Database)", "postgres")  # <-- Add your new line here
}

```

*Note: The second value (`"postgres"`) is the internal ID and must be completely lowercase with no spaces.*

#### Step 2: Add YAML Block to Configuration Generator (`generate_docker_compose`)

Scroll down to the `generate_docker_compose` function and append an `if` block with your container's Docker Compose structure before the file write operation:

```python
    if "postgres" in selected_tools:
        compose_data += """  postgres:
    image: docker.io/library/postgres:16-alpine
    container_name: postgres
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
    ports:
      - "5432:5432"
    volumes:
      - ./data/postgres:/var/lib/postgresql/data:Z
    restart: unless-stopped
\n"""

```

> ⚠️ **RHEL 10 / Podman Notice:** If your new container mounts a local directory host volume (like `./data/postgres`), **always append the `:Z` flag** to the volume mapping string. This ensures SELinux labels are correctly assigned by Podman; otherwise, the container will crash with a `Permission denied` error.

---

### Русский

Скрипт `deploy.py` спроектирован декларативно. Если в будущем вы захотите расширить список доступных инструментов (например, добавить MySQL, Redis или Nginx), вам потребуется изменить всего **два места** в коде:

#### Шаг 1: Обновите меню выбора (`TOOLS_MAP`)

Найдите словарь `TOOLS_MAP` в самом начале скрипта и добавьте туда новую строку со следующим порядковым номером:

```python
TOOLS_MAP = {
    1: ("Prometheus", "prometheus"),
    # ...
    10: ("Alertmanager (Менеджер алертов)", "alertmanager"),
    11: ("PostgreSQL (База данных)", "postgres")  # <-- Ваша новая строка
}

```

*Примечание: Второй параметр (`"postgres"`) — это внутренний идентификатор, он должен быть написан в нижнем регистре и без пробелов.*

#### Шаг 2: Добавьте блок YAML в генератор конфигурации (`generate_docker_compose`)

Внутри функции `generate_docker_compose` найдите блок проверок и добавьте условие `if` с конфигурацией вашего контейнера для Docker Compose (перед финальной записью файла):

```python
    if "postgres" in selected_tools:
        compose_data += """  postgres:
    image: docker.io/library/postgres:16-alpine
    container_name: postgres
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
    ports:
      - "5432:5432"
    volumes:
      - ./data/postgres:/var/lib/postgresql/data:Z
    restart: unless-stopped
\n"""

```

> ⚠️ **Важно для RHEL 10 / Podman:** Если ваш новый контейнер монтирует локальные папки хоста (например, `./data/postgres`), **всегда добавляйте флаг `:Z**` в конце пути монтирования volume. Это необходимо для правильной разметки контекстов безопасности SELinux в Podman, иначе контейнер упадет с ошибкой `Permission denied`.

```

```
