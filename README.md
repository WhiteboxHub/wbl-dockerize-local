#  WBL Docker Setup

This project runs the **WBL Backend (FastAPI)**, **Frontend (Next.js)**, and **MySQL Database** locally using Docker and Docker Compose.

##  Project Structure

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

##  Requirements

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Docker Compose](https://docs.docker.com/compose/install/)

##  Run the App Locally

```
docker-compose build
docker-compose up
```

Frontend → http://localhost:3000  
Backend → http://localhost:8000/docs  
MySQL → localhost:3307 (root/root)

##  Database Details

| Key | Value |
|-----|--------|
| Host | localhost |
| Port | 3307 |
| Database | whitebox |
| Username | root |
| Password | root |

Connect to the DB:
```
docker exec -it wbl-db mysql -u root -p
```
Password: root

```
USE whitebox;
SHOW TABLES;
```

##  Environment Variables

### Backend (`wbl-backend/.env.local`)
```
DB_HOST=db
DB_PORT=3306
DB_USER=root
DB_PASSWORD=root
DB_NAME=whitebox
BACKEND_PORT=8000
```

### Frontend (`wbl-frontend/.env.local`)
```
NEXT_PUBLIC_API_URL=http://localhost:8000
```

## Common Commands

| Action | Command |
|---------|----------|
| Build containers | docker-compose build |
| Run all containers | docker-compose up |
| Stop containers | docker-compose down |
| Reset database | docker-compose down -v |
| Access MySQL shell | docker exec -it wbl-db mysql -u root -p |

##  Updating the Database Schema

Add a new column:
```
ALTER TABLE vendor_contact_extracts ADD COLUMN not_useful VARCHAR(255);
```
Make it persistent:
Add the same line to `sql/init.sql`.

##  Backend Notes

- Built with FastAPI + SQLAlchemy
- To expose new fields, update ORM + Pydantic models
- Restart backend after changes:
```
docker-compose restart backend
```

##  Frontend Notes

- Built with Next.js 14
- Update React components to display new fields (e.g., `vendor.not_useful`)

##  Reset Everything (Fresh Start)

```
docker-compose down -v
docker-compose build
docker-compose up
```

##  Tips

- No need for MySQL Workbench — Docker runs MySQL locally
- If password fails, use `docker-compose down -v` to reset
- Use `.env.local` for dev, `.env.production` for deployments

##  License

Internal setup for **WhiteBoxHub** local development.
