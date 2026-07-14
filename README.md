<p align="center">
  <img src="https://img.shields.io/badge/FastAPI-0.110-009688?style=for-the-badge&logo=fastapi" alt="FastAPI"/>
  <img src="https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react" alt="React"/>
  <img src="https://img.shields.io/badge/XGBoost-ML-150458?style=for-the-badge" alt="XGBoost"/>
  <img src="https://img.shields.io/badge/PostgreSQL-16-316192?style=for-the-badge&logo=postgresql" alt="PostgreSQL"/>
  <img src="https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker" alt="Docker"/>
  <img src="https://img.shields.io/github/stars/roohan-514/ICU-MONITORING-SYSTEM?style=for-the-badge" alt="Stars"/>
</p>

<h1 align="center">AI-Powered ICU Monitoring System</h1>

<p align="center">
  <em>Real-time ICU patient monitoring with XGBoost risk prediction, clinical alerts, and automated reports.</em><br/>
  FastAPI backend · React dashboard · WebSocket streaming · Docker deployment
</p>

---

## Overview

A full-stack clinical intelligence system that streams simulated patient vitals in real-time, predicts deterioration risk using XGBoost, triggers clinical alerts, and generates AI-powered summary reports.

Built for healthcare AI research, medical system prototyping, and demonstrating production-grade full-stack ML integration.

---

## Features

- **Real-time vital streaming** — WebSocket-based live vitals from simulated ICU patients
- **XGBoost risk prediction** — ML model predicts patient deterioration (low/medium/high/critical)
- **Clinical alert engine** — Automatic alerts for critical vital sign thresholds
- **AI-generated reports** — Automated clinical summaries with recommendations
- **Interactive dashboard** — React-based real-time monitoring with Recharts
- **Patient management** — Full CRUD for ICU patients
- **Docker support** — One-command deployment with docker-compose

---

## Architecture

```
backend/                    FastAPI Backend
  app/
    main.py                 Entry point with WebSocket support
    database.py             PostgreSQL connection (SQLAlchemy)
    models.py               Patient, Vitals, Alert, Report models
    schemas.py              Pydantic request/response schemas
    websocket_manager.py    WebSocket connection manager
    risk_engine.py          XGBoost risk prediction engine
    routes/
      patients.py           Patient CRUD
      vitals.py             Vitals & dashboard API
      reports.py            AI report generation
    ml/
      train_model.py        Model training script
      heart_model.pkl       Trained XGBoost model
  simulator/
    icu_simulator.py        Patient data simulator
  requirements.txt
  Dockerfile

frontend/                   React Frontend (Vite + Tailwind)
  src/
    App.jsx
    pages/
      Dashboard.jsx         Main monitoring dashboard
    components/
      PatientCard.jsx       Patient status card
      VitalChart.jsx        Real-time vital charts (Recharts)
      AlertPanel.jsx        Clinical alerts panel
    services/
      websocket.js          WebSocket client

docker-compose.yml          Backend + Frontend + PostgreSQL
```

---

## Quick Start (Docker)

```bash
docker compose up --build
```

Then open:
- **Dashboard:** http://localhost:5173
- **Backend API:** http://localhost:8000
- **Swagger Docs:** http://localhost:8000/docs

---

## Local Development

### 1. Start PostgreSQL

```bash
docker run -d --name icu-postgres \
  -e POSTGRES_USER=icu_user \
  -e POSTGRES_PASSWORD=icu_password \
  -e POSTGRES_DB=icu_monitoring \
  -p 5432:5432 \
  postgres:16-alpine
```

### 2. Backend

```bash
cd backend
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements.txt
python -m app.ml.train_model
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### 3. Frontend

```bash
cd frontend
npm install
npm run dev
```

---

## API Endpoints

### Patients

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/patients/` | List all patients |
| POST | `/patients/` | Admit a patient |
| GET | `/patients/{id}` | Get patient details |
| PUT | `/patients/{id}` | Update patient |
| DELETE | `/patients/{id}` | Discharge patient |
| GET | `/patients/{id}/stats` | Patient statistics |

### Vitals & Dashboard

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/vitals/{patient_id}` | Vital history |
| GET | `/vitals/{patient_id}/latest` | Latest vitals |
| GET | `/dashboard/stats` | Dashboard statistics |
| GET | `/dashboard/risk-distribution` | Risk level distribution |
| WS | `/ws/dashboard` | Real-time dashboard stream |
| WS | `/ws/patient/{id}` | Real-time patient stream |

### Reports

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/reports/generate` | Generate AI clinical report |
| GET | `/reports/patient/{id}` | Patient's reports |
| GET | `/reports/{id}` | Get specific report |

---

## WebSocket Protocol

Connect to `ws://localhost:8000/ws/dashboard`.

**Server messages:**
- `vital_update` — New vitals for a patient
- `alert` — Clinical alert triggered
- `patient_update` — Patient status change

**Client actions:**
```json
{"action": "ping"}
{"action": "stats"}
```

---

## ML Model

XGBoost classifier trained on synthetic clinical data.

**Features:** Age, Heart Rate, Blood Pressure, Respiratory Rate, SpO2, Temperature, GCS Score

**Output:** Risk score (0-1) → low, medium, high, critical

To retrain:
```bash
cd backend
python -m app.ml.train_model
```

---

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `DATABASE_URL` | postgresql://icu_user:icu_password@localhost:5432/icu_monitoring | PostgreSQL connection |
| `API_PORT` | 8000 | Backend port |
| `VITE_API_URL` | http://localhost:8000 | Backend URL (frontend) |
| `VITE_WS_URL` | ws://localhost:8000 | WebSocket URL (frontend) |

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | FastAPI, SQLAlchemy, PostgreSQL, WebSockets |
| ML | XGBoost, scikit-learn, NumPy, Pandas |
| Frontend | React, Vite, Tailwind CSS, Recharts, Lucide Icons |
| Infrastructure | Docker, Docker Compose |

---

## License

MIT
