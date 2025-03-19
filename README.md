# Tuvote Platform – Backend, Frontend & Monitoring

This repository contains the **Tuvote platform**, a full-stack application with a Node.js/Express backend, a React frontend, and integrated monitoring.  
It is designed as a **portfolio-ready project** to showcase DevOps, CI/CD, and observability skills.

---

## 🚀 Features
- **Backend**: Node.js + Express + MongoDB (with authentication, voting, elections, candidates, and dashboard APIs).
- **Frontend**: React application served via Nginx.
- **Dockerized**: Both backend and frontend run inside Docker containers.
- **CI/CD**: GitHub Actions pipeline builds & tests the app.
- **Monitoring**:
  - Prometheus collects application metrics (via `/metrics` endpoint in backend).
  - Grafana visualizes metrics in dashboards.

---

## 🐳 Docker Setup

### Backend (Express + MongoDB)
- Dockerfile builds the backend app.
- Exposes port **3001**.
- Uses `prom-client` to expose metrics at `/metrics`.

### Frontend (React + Nginx)
- Dockerfile builds the React app.
- Served by Nginx on port **81** (mapped to host `8081`).

---

## 🧩 Docker Compose
`docker-compose.yml` ties everything together:

- **backend** → `tuvote-backend`
- **frontend** → `tuvote-frontend`
- **prometheus** → scrapes backend metrics on port `3001`
- **grafana** → visualizes Prometheus data on port `3000`

Example services:

```yaml
services:
  backend:
    build: ./tuvote-backend
    ports:
      - "3001:3001"

  frontend:
    build: ./tuvote-frontend
    ports:
      - "8081:81"
    depends_on:
      - backend

  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    depends_on:
      - prometheus
````

---

## ⚙️ CI/CD (GitHub Actions)

* A **pipeline** is configured in `.github/workflows/ci.yml`.
* On every push:

  1. Install dependencies.
  2. Run tests.
  3. Build Docker images.
  4. Ensure successful build before deployment.

This ensures the repository always stays **green** and deployable.

---

## 📊 Monitoring

### Prometheus

* Configured via `prometheus.yml`.
* Scrapes backend metrics every 5s.
* Accessible at: [http://localhost:9090](http://localhost:9090)

### Grafana

* Runs on port **3000**.
* First-time setup requires adding Prometheus as a datasource:

  * URL: `http://tuvote-prometheus:9090`
* Dashboards can be created from UI (custom panels for request counts, response times, error rates, etc.).

---

## 🛠️ Development Workflow

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/tuvote-platform.git
   cd tuvote-platform
   ```

2. Start containers:

   ```bash
   docker-compose up --build
   ```

3. Access services:

   * **Frontend** → [http://localhost:8081](http://localhost:8081)
   * **Backend API** → [http://localhost:3001](http://localhost:3001)
   * **Prometheus** → [http://localhost:9090](http://localhost:9090)
   * **Grafana** → [http://localhost:3000](http://localhost:3000)

---

## 🎯 Why This Project Matters (Portfolio / CV)

This project demonstrates **real-world DevOps & full-stack engineering skills**:

* Building **Dockerized microservices** (backend + frontend).
* Orchestrating with **Docker Compose**.
* Implementing **CI/CD pipelines** for automation.
* Adding **observability** (Prometheus + Grafana).
* Presenting metrics and dashboards like a production system.

It is ideal for showcasing to recruiters and companies:

> “I can design, build, containerize, automate, and monitor a full-stack system end-to-end.”

---

## 📌 Next Steps

* (Optional) Add **Loki** for centralized logging with Grafana.
* (Optional) Provision Grafana dashboards automatically via config files.
* Deploy to a **VPS or cloud provider** for a live demo.

---

```

---


