# 📂 Codebase Folder Structures

### 1. `/server` Layout
```text
MedMemory/
├── app/
│   ├── __init__.py
│   ├── main.py                  # FastAPI instantiation & middleware setup
|   |
|   ├── core/
│   |    ├── config.py           # Environment configurations (Pydantic Settings)
│   |    ├── logging.py          # Centralized Logging
│   |    ├── db.py               # Centralized Prisma Connection pool
│   |    ├── qdrant.py           # Qdrant Client Connection management
│   |    └── exceptions.py       # Centralized Exception handling
│   │
│   ├── api/                     # Controller layer (Routes)
│   │   ├── __init__.py
│   │   ├── router.py            # Global router registration hub
│   │   ├── deps.py              # Retrieves `patient_id` from Global `app`
│   │   ├── middleware.py        # Authorization Middleware
│   │   ├── v1/
│   │   │   ├── endpoints/
│   │   │   │   ├── auth.py      # Authentication management
│   │   │   │   ├── ingestion.py # Endpoints for document uploading & processing
│   │   │   │   ├── chat.py      # Interactive query & LangGraph chat sockets
│   │   │   │   └── timeline.py  # Historical chronological rendering data
│   │   │   └── __init__.py
│   │
│   ├── schemas/                 # Pydantic Schemas (Request/Response contracts)
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   ├── ingestion.py
│   │   ├── timeline.py
│   │   └── chat.py
│   │
│   ├── services/                     # Core Business Logic Layer
│   │   ├── __init__.py
│   │   │
│   │   ├── auth/                     # Authentication & Authorization Services
│   │   │   ├── service.py            # Patient registration & login workflows
│   │   │   ├── jwt.py                # JWT access token generation & validation
│   │   │   └── crypto.py             # Password hashing & verification
│   │   │
│   │   ├── ingestion/                # Medical Document Ingestion Pipeline
│   │   │   ├── main.py               # LangChain pipeline orchestration (8-step workflow)
│   │   │   ├── validate_store.py     # File validation & secure local storage
│   │   │   ├── ocr.py                # OCR & text extraction (PDFs / Images)
│   │   │   ├── normalizer.py         # Text cleaning, normalization & typo correction
│   │   │   ├── ner.py                # Clinical NER & entity linking (sciSpacy)
│   │   │   ├── extraction.py         # Structured medical data extraction using LLM
│   │   │   ├── store_sql.py          # PostgreSQL persistence layer
│   │   │   └── store_qdrant.py       # Embedding generation & Qdrant indexing
│   │   │
│   │   ├── chat/                     # AI Medical Memory Chat Pipeline
│   │   │   ├── main.py               # LangGraph workflow & SSE streaming engine
│   │   │   ├── query_validate.py     # Query validation & sanitization
│   │   │   ├── intent.py             # Query intent classification (Semantic / Hybrid)
│   │   │   ├── retriever.py          # Context retrieval from Qdrant & Postgres
│   │   │   └── generator.py          # Prompt assembly & LLM response generation
│   │   │
│   │   └── timeline/                 # Patient Health Timeline Services
│   │       └── main.py               # Chronological medical history retrieval
│   │
│   └── prisma/                  # Prisma schema management configurations
│       └── schema.prisma        # Database entity maps
│
├── .env.example
├── setup.sh
├── setup.ps1
├── Dockerfile
├── docker-compose.yml
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
