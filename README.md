# 🏨 BookingHotels API

A REST API for hotel booking built with **FastAPI**, featuring asynchronous database access, Redis caching, Celery queues, and Prometheus/Grafana monitoring 🚀  
The repository is prepared for running via **Docker Compose** 🐳.

## ✨ Features
- 🔍 Hotel search by location and dates with period validation and response caching.
- 🔐 Registration, login, and logout with JWT issuance and **HttpOnly** cookies.
- 🧾 CRUD operations for bookings (create, list user bookings, delete).
- 🏢 Dedicated endpoints for hotel details and room lists.
- 🖼️ Hotel image uploads with background resizing via Celery.
- 🛠️ Admin panel powered by **SQLAdmin** for users, hotels, rooms, and bookings.
- 📊 Metrics and experimental load endpoints for Prometheus/Grafana.

## 🧰 Tech Stack
- 🐍 Python 3.9, FastAPI, Pydantic, Starlette
- 🗄️ SQLAlchemy 2.0 (async) + asyncpg, Alembic migrations
- ⚡ Redis + fastapi-cache2
- 🔄 Celery + Flower
- 🐘 PostgreSQL
- 📈 Prometheus + Grafana
- 🐳 Docker, Docker Compose
- 🧪 Pytest for unit and integration tests

## 📋 Prerequisites
- Docker and Docker Compose
- (Assumed) `.env` / `.env-non-dev` with DB/Redis/SMTP/JWT settings according to `app/config.py`:
  - `MODE`, `LOG_LEVEL`, `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASS`, `DB_NAME`
  - `TEST_DB_*`, `SMTP_HOST/PORT/USER/PASS`, `REDIS_HOST/PORT`, `SECRET_KEY`, `ALGORITHM`

## ▶️ Setup & Run
1. Copy environment variables (assumed):  
   `cp .env.example .env-non-dev` and fill in the values ✍️
2. Build and start services:
   - `docker compose build`
   - `docker compose up`
3. The API will be available at:  
   `http://localhost:7777/api/v1`  
   (note `root_path="/api"` and API versioning).
4. Alembic migrations run inside the container before startup (see `docker/app.sh`) 🧱

### 💻 Local Run Without Docker (assumed)
1. Install dependencies: `pip install -r requirements.txt`
2. Configure PostgreSQL and Redis, fill in `.env`.
3. Apply migrations: `alembic upgrade head`
4. Start the app:  
   `uvicorn app.main:app --reload --port 8000`

## 📌 Usage Examples
- **Register**:
  - `POST /api/v1/auth/register`  
    Body: `{"email":"user@example.com","password":"secret"}`
- **Login**:
  - `POST /api/v1/auth/login` → `{"access_token": "..."}`
- **Get hotels by location/dates**:
  - `GET /api/v1/hotels/{location}?date_from=2025-01-01&date_to=2025-01-07`
- **Create a booking** (authorization required):
  - `POST /api/v1/bookings`  
    Body: `{"room_id":1,"date_from":"2025-01-01","date_to":"2025-01-03"}`

## 🗂️ Project Structure
- `app/main.py` — entry point, routers, CORS, API versioning, cache, metrics, admin panel.
- `app/users` — authentication, schemas, dependencies, DAO.
- `app/hotels`, `app/bookings`, `app/images` — domain routers/DAO/schemas.
- `app/tasks` — Celery, background tasks, and email templates.
- `app/database.py`, `app/dao` — DB connection and base data access layer.
- `app/migrations` — Alembic migrations.
- `docker/`, `Dockerfile`, `docker-compose.yml` — containerization.
- `tests/` — unit and integration tests.

## 🧪 Testing
- Run tests: `pytest`  
  (assumed: configured test DB and Redis, see `TEST_DB_*`).

## 📊 Observability & Ops
- Metrics: `GET /api/v1/metrics` (Prometheus format).
- Load/diagnostic endpoints: `GET /api/v1/prometheus/*`.
- Flower UI: `http://localhost:5555` (default in docker-compose).
- Grafana: `http://localhost:3000`, Prometheus: `http://localhost:9090`.
- Static files: mounted at `/static`.

## 📝 Notes
- Email notifications and Sentry are declared but may be disabled by default (assumed).
- For production, configure secure values for `SECRET_KEY`, `ALGORITHM`, CORS `allow_origins`,
  and allocate proper resources for Celery, Redis, and the database 🔒.
