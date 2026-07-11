# 📂 Codebase Folder Structures

### 1. `/server` Layout
```text
server/
├── app/
│   ├── api/          # FastAPI Routes (Ingestion, Queries)
│   ├── core/         # Configs, Security, Database Connections
│   ├── models/       # Postgres SQLAlchemy Schemas
│   ├── services/     # RAG Pipelines, Vector Computations, Document Parsing
│   └── main.py       # Server entry point
├── requirements.txt
└── README.md
```
---

### 2. `/android-app` Layout
```text
android-app/
├── src/
│   ├── components/   # Timeline UI components, charts, upload pickers
│   ├── hooks/        # State updates, API query triggers
│   ├── navigation/   # Routing trees
│   └── App.js        # Entry point
├── package.json
└── README.md
```