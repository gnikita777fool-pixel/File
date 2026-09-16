# 2. PostgreSQL + pgAdmin

## Структура проекта

```text
postgres-pgadmin/
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
    container_name: postgres_pgadmin_db
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: admin123
    ports:
      - "5433:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  pgadmin:
    image: dpage/pgadmin4:latest
    container_name: pgadmin
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@example.com
      PGADMIN_DEFAULT_PASSWORD: admin123
    ports:
      - "8081:80"
    depends_on:
      - postgres
    volumes:
      - pgadmin_data:/var/lib/pgadmin

volumes:
  postgres_data:
  pgadmin_data:
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
cd postgres-pgadmin
```

Запустить контейнеры:

```bash
docker compose up --build
```

После запуска pgAdmin будет доступен по адресу:

```text
http://localhost:8081
```

## Вход в pgAdmin

Использовать следующие данные:

| Параметр | Значение                                      |
| -------- | --------------------------------------------- |
| Email    | [admin@example.com](mailto:admin@example.com) |
| Password | admin123                                      |

## Подключение PostgreSQL

После входа в pgAdmin необходимо создать подключение к PostgreSQL.

| Параметр | Значение |
| -------- | -------- |
| Host     | postgres |
| Port     | 5432     |
| Database | appdb    |
| Username | admin    |
| Password | admin123 |

> **Важно:** внутри Docker-сети используется имя сервиса `postgres` и порт `5432`.
>
> Порт `5433` используется только для подключения к PostgreSQL с компьютера пользователя.