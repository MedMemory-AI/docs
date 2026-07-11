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
│   |    ├── security.py         # Auth guardrails, JWT validations, hashing
│   |    ├── logging.py          # Centralized Logging
│   |    └── exceptions.py       # Centralized Exception handling
│   │
│   ├── api/                     # Controller layer (Routes)
│   │   ├── __init__.py
│   │   ├── router.py            # Global router registration hub
│   │   ├── v1/
│   │   │   ├── endpoints/
│   │   │   │   ├── ingestion.py # Endpoints for document uploading & parsing
│   │   │   │   ├── chatbot.py   # Interactive query & LangGraph chat sockets
│   │   │   │   └── timeline.py  # Historical chronological rendering data
│   │   │   └── __init__.py
│   │
│   ├── schemas/                 # Pydantic Schemas (Request/Response contracts)
│   │   ├── __init__.py
│   │   ├── patient.py
│   │   ├── clinical.py          # Strict Pydantic models for LLM structured output
│   │   └── chat.py
|   |
|   ├── models/                  # Prisma Schema models
│   |    ├── postgres.py
│   |    └── qdrant.py
│   │
│   ├── services/                # Business Logic Engine Layer
│   │   ├── __init__.py
│   │   ├── ingestion/           # Ingestion microservice cluster
│   │   │   ├── parser.py        # Docling layout processing & parsing
│   │   │   ├── processor.py     # Clean & normalize, spaCy sentence segmentation
│   │   │   ├── ner.py           # scispaCy & MedCAT medical NER tagging
│   │   │   ├── extractor.py     # Local LLM structured schema generator
│   │   │   └── storage.py       # Dual-write driver to Postgres (Prisma) & Qdrant
│   │   │
│   │   ├── chatbot/             # Chatbot Multi-Agent pipeline
│   │   │   ├── graph.py         # LangGraph network architecture mapping
│   │   │   ├── classifier.py    # Local LLM request intent engine
│   │   │   ├── retrievers.py    # Database connection routing (Postgres + Qdrant)
│   │   │   └── generator.py     # Prompt compiler & guardrailed response writer
│   │   │
│   │   └── timeline/
│   │       └── generator.py     # Sequential timeline fact synthesizer
│   │
│   └── prisma/                  # Prisma schema management configurations
│       └── schema.prisma        # Database entity maps
│
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