# WBL Docker Setup

This project runs the WBL Backend (FastAPI), Frontend (Next.js), and MySQL Database locally using Docker and Docker Compose.

---

## Project Structure

```
wbl-docker/
│
├── sql/
│   └── init.sql                     # Initializes MySQL schema & seed data
│
├── wbl-backend/
│   ├── Dockerfile                   # FastAPI backend container
│   ├── requirements.txt             # Python dependencies
│   ├── .env.local                   # Local backend environment variables
│   └── fapi/                        # FastAPI app folder (main.py, routes, models, etc.)
│
├── wbl-frontend/
│   ├── Dockerfile                   # Next.js frontend container
│   ├── package.json                 # Node dependencies
│   ├── .env.local                   # Local frontend environment variables
│   └── src/                         # React components and pages
│
└── docker-compose.yml               # Orchestrates DB + Backend + Frontend
```

---

## Requirements

* Docker Desktop
* Docker Compose

---

## Run the App Locally

```bash
docker-compose build
docker-compose up
```

* Frontend → [http://localhost:3000](http://localhost:3000)
* Backend → [http://localhost:8000/docs](http://localhost:8000/docs)
* MySQL → localhost:3307 (root/root)

---

## Database Details

| Key      | Value     |
| -------- | --------- |
| Host     | localhost |
| Port     | 3307      |
| Database | whitebox  |
| Username | root      |
| Password | root      |

Connect to the DB:

```bash
docker exec -it wbl-db mysql -u root -p
# Password: root

USE whitebox;
SHOW TABLES;
```

---

## Environment Variables

**Backend (wbl-backend/.env.local)**

```
DB_HOST=db
DB_PORT=3306
DB_USER=root
DB_PASSWORD=root
DB_NAME=whitebox
BACKEND_PORT=8000
```

**Frontend (wbl-frontend/.env.local)**

```
NEXT_PUBLIC_API_URL=http://localhost:8000
```

---

## Common Commands

|             Action | Command                                   |
| -----------------: | ----------------------------------------- |
|   Build containers | `docker-compose build`                    |
| Run all containers | `docker-compose up`                       |
|    Stop containers | `docker-compose down`                     |
|     Reset database | `docker-compose down -v`                  |
| Access MySQL shell | `docker exec -it wbl-db mysql -u root -p` |

---

## Database: Export 

* `sql/init.sql` is the sanitized SQL dump (schema + seed data) used to initialize the MySQL container.
* The dump was produced by exporting the production DB and then renaming the sanitized file to `init.sql`.
* MySQL executes `*.sql` files placed in `/docker-entrypoint-initdb.d/` **only on first initialization**. To re-run, remove the DB volume or import manually.

---

## Updating the Database Schema

**Add a new column (example):**

```sql
ALTER TABLE table_name ADD COLUMN column_name VARCHAR(255);
```

**Make it persistent:** add the same `ALTER TABLE` statement (or the table DDL) into `sql/init.sql` so new dev setups include the change.

---

## Backend Notes

* Built with FastAPI + SQLAlchemy
* To expose new fields, update ORM and Pydantic models
* Restart backend after changes:

```bash
docker-compose restart backend
```

---

## Frontend Notes

* Built with Next.js 14
* Update React components to display new fields (e.g., `vendor.not_useful`)

---

## Reset Everything (Fresh Start)

```bash
docker-compose down -v
docker-compose build
docker-compose up
```

---

## Tips

* No need for MySQL Workbench — Docker runs MySQL locally
* If password fails, use `docker-compose down -v` to reset
* Use `.env.local` for dev, `.env.production` for deployments

---

## License

Internal setup for WhiteBoxHub local development.

---

*This README replaces the previous dump-focused doc and includes the local dev setup, DB connection details, and quick commands.*
