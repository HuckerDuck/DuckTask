# EnterPrice - Todo Backend

A school assignment I built to practice enterprise-level backend development.
A REST API for managing todo tasks with user roles, JWT auth, RabbitMQ messaging and Docker.

> **Note:** All code is written by me. I used AI to help structure this README, but reviewed and approved README myself.

---

## What is this?

This was a school assignment focused on building a more "enterprise-style" backend.
The goal was to go beyond a basic CRUD app and work with things like message queues, roles and permissions, and containerization.

Still a work in progress, but the core features are working.

---

## Tech

- Java 17 + Spring Boot
- Spring Security + JWT
- MySQL + Spring Data JPA
- RabbitMQ (message queue)
- Docker + Docker Compose
- Lombok, MapStruct
- Gradle

---

## Features

**Tasks (DuckTasks)**
- Full CRUD on todo tasks
- Each task has a title, description, priority and completed status
- Tasks use UUID as ID
- Timestamps set automatically on create and update
- Tasks are tied to the logged-in user

**Users (Ducks)**
- Register and login
- Roles: USER and ADMIN
- Permissions: READ, WRITE, DELETE
- JWT stored and validated on every request

**Messaging**
- RabbitMQ queue for email notifications
- When a task event happens, a message is sent to `email-queue`
- EmailConsumer listens and handles the message

---

## Endpoints

### Tasks `/api/ducktasks`

```
POST   /api/ducktasks                      - create task
GET    /api/ducktasks                      - get all tasks (current user)
GET    /api/ducktasks/{id}                 - get task by id
PUT    /api/ducktasks/update/{id}          - update task
DELETE /api/ducktasks/delete/{id}          - delete task
PATCH  /api/ducktasks/completedtask/{id}   - mark task as completed
```

### Auth `/api/auth`

```
POST   /api/auth/register
POST   /api/auth/login
```

---

## Project Structure

```
src/
├── advice/                  # Global validation error handling
├── config/
│   └── RabbitConfig.java    # RabbitMQ queue, exchange, binding
├── consumer/
│   └── EmailConsumer.java   # Listens to email-queue
├── security/
│   ├── jwt/                 # JWT filter + utils
│   ├── corsConfig/
│   └── controller/          # Auth endpoints
├── todo_item/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── model/               # DuckTask entity
│   ├── dto/
│   ├── mapper/
│   ├── enums/               # ToDoPriority
│   └── exceptions/
└── user/
    ├── controller/
    ├── service/
    ├── repository/
    ├── model/               # Duck (user) entity
    ├── dto/
    ├── mapper/
    ├── enums/               # DuckRoles, DuckPermissions
    └── exceptions/
```

---

## Run with Docker

The easiest way to run this project is with Docker Compose.
It will start both the backend and RabbitMQ automatically.

```bash
git clone https://github.com/your-username/enterprice-todo-backend.git
cd enterprice-todo-backend
```

Copy the env example and fill in your database details:

```bash
cp .env_example .env
```

Start everything:

```bash
docker-compose up --build
```

API runs on `http://localhost:8080`
RabbitMQ dashboard runs on `http://localhost:15672` (guest / guest)

---

## Run without Docker

You need Java 17+, MySQL and Gradle.

Configure `.env` with your database credentials, then:

```bash
./gradlew bootRun
```

---

## Author

Fredrik — Java developer student based in Stockholm.
