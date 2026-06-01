# Todolist App — Docker Compose Instructions

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed and running
- [Docker Compose](https://docs.docker.com/compose/install/) installed

---

## How It Works

The `docker-compose.yml` defines two services:

| Service       | Description                           | Port mapping |
| ------------- | ------------------------------------- | ------------ |
| `todoapp`     | Django Todolist application           | 8080 → 8000  |
| `mysql-local` | MySQL database with persistent volume | 3036 → 3306  |

- Both services share a private bridge network `hw-db-net`.
- MySQL data is persisted in the Docker named volume `hw-db` (mapped to `/var/lib/mysql` inside the container).
- On startup, the app container automatically runs `python manage.py migrate` before launching the Django dev server.

---

## Running the Containers

### 1. Build images and start in detached mode

```bash
docker-compose up --build -d
```

### 2. Access the application

Open your browser and navigate to:

```
http://localhost:8080
```

---

## Stopping the Containers

### Stop and keep containers (data is preserved)

```bash
docker-compose stop
```

### Stop and remove containers (data volume is preserved)

```bash
docker-compose down
```

### Stop, remove containers **and** delete the persistent volume

```bash
docker-compose down -v
```

---

## Useful Commands

| Task                              | Command                                                       |
| --------------------------------- | ------------------------------------------------------------- |
| View running containers           | `docker-compose ps`                                           |
| View app logs                     | `docker-compose logs todoapp`                                 |
| View DB logs                      | `docker-compose logs mysql-local`                             |
| Follow logs in real time          | `docker-compose logs -f`                                      |
| Restart a single service          | `docker-compose restart todoapp`                              |
| Open a shell in the app container | `docker-compose exec todoapp sh`                              |
| Open MySQL CLI                    | `docker-compose exec mysql-local mysql -u app_user -p app_db` |
