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
