# 🏨 BookingHotels - REST API для бронирования отелей

<div align="center">

![Python](https://img.shields.io/badge/Python-3.9+-blue?style=for-the-badge&logo=python)
![FastAPI](https://img.shields.io/badge/FastAPI-0.92.0-green?style=for-the-badge&logo=fastapi)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-2.0-orange?style=for-the-badge&logo=sqlalchemy)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue?style=for-the-badge&logo=postgresql)
![Docker](https://img.shields.io/badge/Docker-Compose-blue?style=for-the-badge&logo=docker)

**API для бронирования отелей с асинхронной архитектурой, кэшированием, фоновыми задачами и мониторингом**

[Описание](#-описание) • [Возможности](#-возможности) • [Технологии](#-технологии) • [Установка](#-установка) • [Конфигурация](#-конфигурация) • [Структура проекта](#-структура-проекта)

</div>

---

## 📋 Описание

**BookingHotels** — это полнофункциональный REST API для системы бронирования отелей, разработанный на **FastAPI**. Проект демонстрирует современные подходы к разработке веб-приложений: асинхронную работу с базой данных, кэширование ответов, фоновую обработку задач через Celery, а также мониторинг и метрики через Prometheus и Grafana.

### 🎯 Основные функции

- 🔍 **Поиск отелей** — поиск по локации и датам с валидацией периодов и кэшированием ответов
- 🔐 **Аутентификация** — регистрация, вход и выход с JWT токенами в HttpOnly cookies
- 🧾 **Управление бронированиями** — создание, просмотр и удаление бронирований
- 🏢 **Информация об отелях** — детальная информация об отелях и доступных номерах
- 🖼️ **Загрузка изображений** — загрузка изображений отелей с фоновой обработкой через Celery
- 🛠️ **Админ-панель** — полноценная админ-панель на SQLAdmin для управления данными
- 📊 **Мониторинг** — метрики и дашборды через Prometheus и Grafana

---

## ✨ Возможности

### 👤 Для пользователей

- 🔍 **Удобный поиск** — поиск отелей по локации с указанием дат заезда и выезда
- 📅 **Валидация дат** — автоматическая проверка корректности периода бронирования
- 🔐 **Безопасная авторизация** — JWT токены в защищенных HttpOnly cookies
- 📋 **Управление бронированиями** — просмотр всех своих бронирований и возможность отмены
- 📧 **Email-уведомления** — автоматическая отправка подтверждений на email
- ⚡ **Быстрые ответы** — кэширование популярных запросов для повышения производительности

### 🔧 Для администраторов

- 🛠️ **Админ-панель** — удобный интерфейс для управления пользователями, отелями, номерами и бронированиями
- 📊 **Мониторинг** — отслеживание метрик производительности через Prometheus и Grafana
- 🔄 **Фоновые задачи** — мониторинг Celery задач через Flower
- 📥 **Импорт данных** — загрузка данных из CSV файлов в базу данных
- 📈 **Аналитика** — детальная статистика по использованию API

---

## 🛠 Технологии

### Основной стек

- **Python 3.9+** — язык программирования
- **FastAPI 0.92.0** — современный асинхронный веб-фреймворк
- **SQLAlchemy 2.0** — ORM для работы с базой данных (асинхронный режим)
- **PostgreSQL 15** — реляционная база данных
- **asyncpg** — асинхронный драйвер для PostgreSQL
- **Alembic** — система миграций базы данных

### Кэширование и очереди

- **Redis 7** — in-memory хранилище для кэширования
- **fastapi-cache2** — интеграция кэширования с FastAPI
- **Celery 5.2.7** — распределенная очередь задач
- **Flower** — веб-интерфейс для мониторинга Celery

### Мониторинг и наблюдаемость

- **Prometheus** — сбор метрик
- **Grafana** — визуализация метрик и дашборды
- **prometheus-fastapi-instrumentator** — автоматическая инструментация FastAPI

### Контейнеризация и тестирование

- **Docker & Docker Compose** — контейнеризация приложения
- **Pytest** — фреймворк для тестирования
- **pytest-asyncio** — поддержка асинхронных тестов

### Дополнительные библиотеки

- **Pydantic** — валидация данных и схемы
- **python-jose** — работа с JWT токенами
- **bcrypt** — хеширование паролей
- **Pillow** — обработка изображений
- **SQLAdmin** — админ-панель для FastAPI

### Архитектура

- 🏗️ **Модульная структура** — разделение на доменные модули (users, hotels, bookings)
- 🔄 **Асинхронное программирование** — использование async/await для высокой производительности
- 📁 **DAO паттерн** — абстракция доступа к данным
- 🗄️ **ORM-модели** — удобная работа с базой данных через SQLAlchemy
- 🧪 **Тестирование** — unit и integration тесты

---

## 📦 Установка

### Предварительные требования

- Python 3.9 или выше
- Docker и Docker Compose
- PostgreSQL 15 (если запуск без Docker)
- Redis 7 (если запуск без Docker)

### Шаги установки

1. **Клонируйте репозиторий**
```bash
git clone https://github.com/yourusername/BookingHotels.git
cd BookingHotels
```

2. **Создайте файл конфигурации**

   Создайте файл `.env-non-dev` в корне проекта со следующим содержимым:
```env
MODE=DEV
LOG_LEVEL=INFO

# База данных PostgreSQL
DB_HOST=db
DB_PORT=5432
DB_USER=your_db_user
DB_PASS=your_db_password
DB_NAME=booking_db

# Тестовая база данных
TEST_DB_HOST=db
TEST_DB_PORT=5432
TEST_DB_USER=your_test_db_user
TEST_DB_PASS=your_test_db_password
TEST_DB_NAME=test_booking_db

# Redis
REDIS_HOST=redis
REDIS_PORT=6379

# SMTP для отправки email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_email_password

# JWT
SECRET_KEY=your_secret_key_here
ALGORITHM=HS256
```

3. **Запустите через Docker Compose**
```bash
docker compose build
docker compose up
```

4. **API будет доступен по адресу:**
   - API: `http://localhost:7777/api/v1`
   - Документация Swagger: `http://localhost:7777/api/v1/docs`
   - Админ-панель: `http://localhost:7777/admin`
   - Flower (Celery): `http://localhost:5555`
   - Grafana: `http://localhost:3000`
   - Prometheus: `http://localhost:9090`

### 💻 Локальный запуск без Docker

1. **Создайте виртуальное окружение**
```bash
python -m venv venv
```

2. **Активируйте виртуальное окружение**

   *Windows:*
```bash
venv\Scripts\activate
```

   *Linux/Mac:*
```bash
source venv/bin/activate
```

3. **Установите зависимости**
```bash
pip install -r requirements.txt
```

4. **Настройте базу данных**

   Убедитесь, что PostgreSQL и Redis запущены и доступны. Создайте файл `.env` с настройками (см. выше, но используйте `localhost` вместо имен контейнеров).

5. **Примените миграции**
```bash
alembic upgrade head
```

6. **Запустите приложение**
```bash
uvicorn app.main:app --reload --port 8000
```

7. **Запустите Celery worker** (в отдельном терминале)
```bash
celery --app=app.tasks.celery:celery worker -l INFO
```

8. **Запустите Flower** (опционально, в отдельном терминале)
```bash
celery --app=app.tasks.celery:celery flower
```

---

## ⚙️ Конфигурация

### Переменные окружения (`.env-non-dev` или `.env`)

| Переменная | Описание | Обязательно |
|-----------|----------|-------------|
| `MODE` | Режим работы (DEV, TEST, PROD) | ✅ |
| `LOG_LEVEL` | Уровень логирования (INFO, DEBUG, WARNING, ERROR) | ✅ |
| `DB_HOST` | Хост базы данных PostgreSQL | ✅ |
| `DB_PORT` | Порт базы данных | ✅ |
| `DB_USER` | Пользователь базы данных | ✅ |
| `DB_PASS` | Пароль базы данных | ✅ |
| `DB_NAME` | Имя базы данных | ✅ |
| `TEST_DB_*` | Настройки тестовой базы данных | ✅ |
| `REDIS_HOST` | Хост Redis | ✅ |
| `REDIS_PORT` | Порт Redis | ✅ |
| `SMTP_HOST` | SMTP сервер для отправки email | ✅ |
| `SMTP_PORT` | Порт SMTP сервера | ✅ |
| `SMTP_USER` | Email для отправки уведомлений | ✅ |
| `SMTP_PASS` | Пароль от email | ✅ |
| `SECRET_KEY` | Секретный ключ для JWT | ✅ |
| `ALGORITHM` | Алгоритм шифрования JWT (обычно HS256) | ✅ |

### Настройки в `app/config.py`

Все настройки загружаются из переменных окружения через Pydantic `BaseSettings`.

---

## 📁 Структура проекта

```
BookingHotels/
├── app/
│   ├── admin/                    # Админ-панель
│   │   ├── auth.py               # Аутентификация для админки
│   │   └── views.py              # Представления для моделей
│   ├── bookings/                 # Модуль бронирований
│   │   ├── dao.py                # Data Access Object
│   │   ├── models.py             # SQLAlchemy модели
│   │   ├── router.py             # API роутеры
│   │   └── schemas.py            # Pydantic схемы
│   ├── hotels/                   # Модуль отелей
│   │   ├── dao.py
│   │   ├── models.py
│   │   ├── router.py
│   │   ├── schemas.py
│   │   └── rooms/                # Подмодуль номеров
│   │       ├── dao.py
│   │       ├── models.py
│   │       ├── router.py
│   │       └── schemas.py
│   ├── users/                    # Модуль пользователей
│   │   ├── auth.py               # Логика аутентификации
│   │   ├── dao.py
│   │   ├── dependencies.py       # Зависимости FastAPI
│   │   ├── models.py
│   │   ├── router.py
│   │   └── schemas.py
│   ├── images/                   # Загрузка изображений
│   │   └── router.py
│   ├── importer/                 # Импорт данных
│   │   ├── router.py
│   │   └── utils.py
│   ├── tasks/                    # Celery задачи
│   │   ├── celery.py             # Конфигурация Celery
│   │   ├── tasks.py              # Фоновые задачи
│   │   └── email_templates.py    # Шаблоны email
│   ├── prometheus/               # Метрики Prometheus
│   │   └── router.py
│   ├── pages/                    # HTML страницы
│   │   └── router.py
│   ├── static/                   # Статические файлы
│   │   ├── css/
│   │   └── images/
│   ├── templates/                # HTML шаблоны
│   │   ├── auth/
│   │   ├── bookings/
│   │   └── hotels_and_rooms/
│   ├── dao/                      # Базовый DAO
│   │   └── base.py
│   ├── migrations/               # Alembic миграции
│   │   ├── versions/
│   │   └── env.py
│   ├── tests/                    # Тесты
│   │   ├── integration_tests/   # Интеграционные тесты
│   │   └── unit_tests/          # Юнит-тесты
│   ├── config.py                 # Настройки приложения
│   ├── database.py               # Подключение к БД
│   ├── exceptions.py             # Кастомные исключения
│   ├── logger.py                 # Настройка логирования
│   ├── main.py                   # Точка входа приложения
│   └── utils.py                  # Вспомогательные функции
├── docker/                       # Docker скрипты
│   ├── app.sh
│   └── celery.sh
├── docker-compose.yml            # Конфигурация Docker Compose
├── Dockerfile                    # Образ приложения
├── alembic.ini                   # Конфигурация Alembic
├── prometheus.yml                # Конфигурация Prometheus
├── grafana-dashboard.json        # Дашборд Grafana
├── requirements.txt              # Зависимости Python
├── pytest.ini                    # Конфигурация pytest
└── README.md                     # Документация
```

---

## 🎮 Использование

### Основные эндпоинты API

#### 🔐 Аутентификация

- **Регистрация:**
```http
POST /api/v1/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "secretpassword"
}
```

- **Вход:**
```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "secretpassword"
}
```

- **Выход:**
```http
POST /api/v1/auth/logout
```

#### 🔍 Поиск отелей

- **Поиск отелей по локации:**
```http
GET /api/v1/hotels/{location}?date_from=2025-01-01&date_to=2025-01-07
```

- **Получить информацию об отеле:**
```http
GET /api/v1/hotels/id/{hotel_id}
```

- **Получить доступные номера:**
```http
GET /api/v1/hotels/{hotel_id}/rooms?date_from=2025-01-01&date_to=2025-01-07
```

#### 🧾 Бронирования

- **Создать бронирование** (требуется авторизация):
```http
POST /api/v1/bookings
Authorization: Bearer {token}
Content-Type: application/json

{
  "room_id": 1,
  "date_from": "2025-01-01",
  "date_to": "2025-01-03"
}
```

- **Получить все бронирования пользователя:**
```http
GET /api/v1/bookings
Authorization: Bearer {token}
```

- **Удалить бронирование:**
```http
DELETE /api/v1/bookings/{booking_id}
Authorization: Bearer {token}
```

#### 🖼️ Загрузка изображений

- **Загрузить изображение отеля:**
```http
POST /api/v1/images/hotels/{hotel_id}
Authorization: Bearer {token}
Content-Type: multipart/form-data

file: [изображение]
```

### 📚 Документация API

После запуска приложения доступна интерактивная документация:
- **Swagger UI:** `http://localhost:7777/api/v1/docs`
- **ReDoc:** `http://localhost:7777/api/v1/redoc`

---

## 🗄️ База данных

Проект использует PostgreSQL с асинхронным драйвером `asyncpg`. Миграции выполняются автоматически при запуске через Docker или вручную через Alembic.

### Модели данных

#### Users (Пользователи)
- `id` — первичный ключ
- `email` — email пользователя (уникальный)
- `hashed_password` — хешированный пароль
- `is_active` — статус активности
- `is_superuser` — флаг суперпользователя
- `is_verified` — флаг верификации

#### Hotels (Отели)
- `id` — первичный ключ
- `name` — название отеля
- `location` — локация отеля
- `services` — список услуг (JSON)
- `rooms_quantity` — количество номеров
- `image_id` — ID изображения

#### Rooms (Номера)
- `id` — первичный ключ
- `hotel_id` — внешний ключ на отель
- `name` — название номера
- `description` — описание
- `price` — цена за ночь
- `services` — список услуг (JSON)
- `quantity` — количество номеров данного типа
- `image_id` — ID изображения

#### Bookings (Бронирования)
- `id` — первичный ключ
- `room_id` — внешний ключ на номер
- `user_id` — внешний ключ на пользователя
- `date_from` — дата заезда
- `date_to` — дата выезда
- `price` — общая стоимость
- `total_cost` — итоговая стоимость
- `total_days` — количество дней

---

## 🧪 Тестирование

Проект включает unit и integration тесты, написанные с использованием pytest.

### Запуск тестов

```bash
pytest
```

### Запуск с покрытием

```bash
pytest --cov=app --cov-report=html
```

### Структура тестов

- `tests/unit_tests/` — юнит-тесты для отдельных компонентов
- `tests/integration_tests/` — интеграционные тесты API и DAO

---

## 📊 Мониторинг и наблюдаемость

### Prometheus

Метрики доступны по адресу:
- **Метрики:** `http://localhost:7777/api/v1/metrics`
- **Prometheus UI:** `http://localhost:9090`

### Grafana

Визуализация метрик через дашборды:
- **Grafana UI:** `http://localhost:3000`
- Дашборд конфигурации находится в `grafana-dashboard.json`

### Flower

Мониторинг Celery задач:
- **Flower UI:** `http://localhost:5555`

---

## 🔒 Безопасность

- ✅ Пароли хешируются с использованием bcrypt
- ✅ JWT токены хранятся в HttpOnly cookies
- ✅ Валидация данных через Pydantic
- ✅ CORS настроен для безопасных запросов
- ✅ Секретные ключи хранятся в переменных окружения
- ✅ Админ-панель защищена аутентификацией

---

## 🚀 Развертывание

### Production настройки

Для production окружения рекомендуется:

1. **Настройте безопасные значения:**
   - Используйте сложный `SECRET_KEY`
   - Настройте CORS `allow_origins` только для ваших доменов
   - Используйте сильные пароли для БД и Redis

2. **Оптимизируйте ресурсы:**
   - Настройте количество воркеров Gunicorn
   - Настройте пулы соединений к БД
   - Выделите достаточно ресурсов для Celery

3. **Настройте мониторинг:**
   - Включите Sentry для отслеживания ошибок
   - Настройте алерты в Grafana
   - Настройте логирование

4. **Используйте HTTPS:**
   - Настройте reverse proxy (nginx)
   - Используйте SSL сертификаты

---

## 📝 Лицензия

Этот проект создан для портфолио. Вы можете использовать его как основу для своих проектов.

---

## 👨‍💻 Автор

Разработано в рамках учебного курса для демонстрации навыков разработки REST API на FastAPI с использованием современных технологий и практик.

---

## 🙏 Благодарности

- [FastAPI](https://fastapi.tiangolo.com/) — современный веб-фреймворк для Python
- [SQLAlchemy](https://www.sqlalchemy.org/) — мощная ORM для Python
- [Celery](https://docs.celeryq.dev/) — распределенная очередь задач
- [Prometheus](https://prometheus.io/) — система мониторинга
- [Grafana](https://grafana.com/) — платформа для визуализации данных

---

<div align="center">

**⭐ Если проект был полезен, поставьте звезду! ⭐**

Made with ❤️ using Python and FastAPI

</div>
