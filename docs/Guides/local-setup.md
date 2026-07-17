# 💻 Local Setup Guide

This guide explains how to run MedMemory locally using either:

1. **Docker Setup (Recommended)**
2. **Manual Setup**

---

# 📋 Prerequisites

Before starting, ensure the following are installed:

- Python 3.10+
- Docker Desktop
- Git
- Ollama

---

# 🚀 Option 1: Docker Setup (Recommended)

This is the quickest way to get MedMemory running locally.

## 1. Clone Repository

```bash
git clone https://github.com/MedMemory-AI/MedMemory.git
cd MedMemory
```

---

## 2. Setup Environment File

Create a ***`.env`*** file in the project root and Copy from ***`.env.example`***, then Set your values.

---

## 3. Run Setup Script

### Linux / macOS

```bash
chmod +x setup.sh
./setup.sh
```

### Windows (PowerShell)

```powershell
Set-ExecutionPolicy Bypass -Scope Process
.\setup.ps1
```

---

# 🛠 Option 2: Manual Setup

Follow these steps if you prefer running services manually.

---

## 1. Clone Repository

```bash
git clone https://github.com/MedMemory-AI/MedMemory.git
cd MedMemory
```

---

## 2. Create Python Virtual Environment

### Windows

```powershell
python -m venv venv
venv\Scripts\activate
```

### Linux / macOS

```bash
python -m venv venv
source venv/bin/activate
```

---

## 3. Install Python Dependencies

```bash
pip install -r requirements.txt
```

---

## 4. Start PostgreSQL

```bash
docker run -d \
  --name medmemory-postgres \
  -p 5432:5432 \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=local_secret \
  -e POSTGRES_DB=medmemory \
  postgres:16
```

---

## 5. Start Qdrant

```bash
docker run -d \
  --name medmemory-qdrant \
  -p 6333:6333 \
  -p 6334:6334 \
  -v qdrant_storage:/qdrant/storage \
  qdrant/qdrant:latest
```

---

## 6. Install Tesseract OCR

### Windows

```powershell
winget install -e --id UB-Mannheim.TesseractOCR
```

### Linux

```bash
sudo apt install tesseract-ocr
```

### macOS

```bash
brew install tesseract
```

---

## 7. Setup Ollama

Install Ollama:

https://ollama.com

Pull required models:

```bash
ollama pull qwen2.5:3b
ollama pull mxbai-embed-large
```

Verify installation:

```bash
ollama list
```

---

## 8. Configure Environment Variables

Create a ***`.env`*** file in the project root and Copy from ***`.env.example`***, then Set your values.

---

## 9. Create Database Schema

```bash
prisma db push --schema=app/prisma/schema.prisma
```

---

## 10. Start FastAPI Server

```bash
uvicorn app.main:app --reload --port 8000
```

---

## 11. Open Dashboards

### Swagger API Documentation

```text
http://localhost:8000/docs
```

### PostgreSQL Dashboard

```bash
npx prisma studio --url="postgresql://postgres:local_secret@localhost:5432/medmemory"
```

### Qdrant Dashboard

```text
http://localhost:6333/dashboard
```

---

# ✅ Verify Installation

The following services should now be available:

| Service | URL |
|----------|------|
| FastAPI API | http://localhost:8000 |
| Swagger Docs | http://localhost:8000/docs |
| PostgreSQL | localhost:5432 |
| Qdrant Dashboard | http://localhost:6333/dashboard |
| Ollama | http://localhost:11434 |

---

# 🧹 Useful Commands

### View Backend Logs

```bash
docker compose logs -f backend
```

### View Ollama Logs

```bash
docker compose logs -f ollama
```

### Restart Backend

```bash
docker compose restart backend
```

### Stop All Services

```bash
docker compose down
```
