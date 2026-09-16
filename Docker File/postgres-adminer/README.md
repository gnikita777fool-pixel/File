# Docker-проекты: PostgreSQL + Adminer, pgAdmin и CloudBeaver

В рамках задания необходимо самостоятельно создать три проекта с использованием `Dockerfile`:

1. PostgreSQL + Adminer
2. PostgreSQL + pgAdmin
3. PostgreSQL + CloudBeaver

Каждый проект располагается в отдельной директории и может запускаться независимо от остальных.

---

# 1. PostgreSQL + Adminer

## Структура проекта

```text
postgres-adminer/
├── Dockerfile
├── docker-compose.yml
└── init.sql
```

## Dockerfile

```dockerfile
FROM postgres:16

ENV POSTGRES_DB=appdb
ENV POSTGRES_USER=admin
ENV POSTGRES_PASSWORD=admin123

COPY init.sql /docker-entrypoint-initdb.d/
```

## docker-compose.yml

```yaml
services:
  postgres:
    build: .
    container_name: postgres_adminer_db
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: admin123
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  adminer:
    image: adminer:latest
    container_name: adminer
    ports:
      - "8080:8080"
    depends_on:
      - postgres

volumes:
  postgres_data:
```

## init.sql

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL
);

INSERT INTO users (name, email)
VALUES
    ('Ivan', 'ivan@example.com'),
    ('Anna', 'anna@example.com');
```

## Запуск

Перейти в директорию проекта:

```bash
cd postgres-adminer
```

Запустить контейнеры:

```bash
docker compose up --build
```

После запуска Adminer будет доступен по адресу:

```text
http://localhost:8080
```

## Данные для подключения

В Adminer необходимо указать:

| Параметр | Значение   |
| -------- | ---------- |
| System   | PostgreSQL |
| Server   | postgres   |
| Username | admin      |
| Password | admin123   |
| Database | appdb      |

---

