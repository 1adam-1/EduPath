# EduPath

EduPath is a microservices-based education/analytics web application designed to help educators monitor student performance, identify **at‑risk students**, view **dashboard analytics**, and generate **personalized recommendations** using a machine-learning service.

## What the app does

EduPath is composed of multiple services that work together:

- **Frontend web app (React + Vite)**  
  A teacher-facing UI with pages for:
  - Login / Sign up / Forgot password
  - Dashboard
  - Classes & class details
  - At‑risk students
  - Analytics
  - Settings

- **Backend (Spring Boot)**  
  Provides core REST APIs, authentication/security, and database access (JPA). It’s set up to use **MySQL** (and includes dependencies for JWT + Spring Security).

- **Gateway (Spring Cloud Gateway)**  
  A single entry point for routing requests to internal services (backend + ML service).

- **Machine Learning service (FastAPI)**  
  Exposes AI endpoints used by the app for predictions, analytics, and recommendations, including:
  - `GET /ai/at-risk-students` – list at‑risk students
  - `POST /ai/at-risk-students` – predict at‑risk students from student input data
  - `GET /ai/dashboard/analytics` – analytics data for the dashboard
  - `POST /ai/recommendations` – generate personalized recommendations
  - `GET /ai/health` – health check

## Tech stack

- **Frontend:** React 18 + Vite, Radix UI components
- **Backend:** Java 17, Spring Boot (Web, JPA, Security), JWT (jjwt), MySQL
- **Gateway:** Spring Boot + Spring Cloud Gateway (WebMVC)
- **ML service:** Python FastAPI + scikit-learn, pandas, numpy
- **Database:** MySQL 8
- **DevOps:** Docker Compose, Jenkins pipeline (build + spin up stack to verify containers run)

## Project structure

- `frontendweb/` – React/Vite frontend
- `backend/` – Spring Boot backend service
- `gateway/` – Spring Cloud Gateway service
- `machine_learning/` – FastAPI ML microservice
- `docker-compose.yml` – local orchestration for the full stack
- `Jenkinsfile` – CI pipeline that builds and tests the Docker composition

## Run with Docker (recommended)

### Prerequisites
- Docker + Docker Compose

### Start everything
```bash
docker-compose up --build
```

### Services & ports (from `docker-compose.yml`)
- **Gateway:** `http://localhost:8080`
- **Backend:** `http://localhost:8081` (mapped from container port 8080)
- **ML service:** `http://localhost:8000`
- **Frontend:** `http://localhost:5173`
- **MySQL:** `localhost:3306` (root password: `root`, database: `edupath`)

### Stop
```bash
docker-compose down
```

## Run locally (without Docker)

### Frontend
```bash
cd frontendweb
npm i
npm run dev
```

### ML service
```bash
cd machine_learning
pip install -r requirements.txt
python main.py
```

### Backend / Gateway
Both backend and gateway are Maven/Spring Boot projects. From each folder:
```bash
./mvnw spring-boot:run
```

## Notes
- The frontend includes screens for auth + teacher dashboard workflows (classes, at‑risk list, analytics).
- The ML service is responsible for predictions/analytics/recommendations and is designed to be called by the rest of the system via the gateway.
