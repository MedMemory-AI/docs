# 🛠️ Tech Stack Overview

| System Layer | Technology | Purpose |
| :--- | :--- | :--- |
| ***Backend API Server*** | FastAPI (Python) | Async APIs, authentication, ingestion, timeline, and chat services |
| ***Mobile Application*** | React Native | Cross-platform patient-facing mobile application |
| ***Workflow Orchestration*** | LangGraph | Stateful AI workflow orchestration and routing |
| ***AI Framework*** | LangChain | LLM integrations, retrieval, embeddings, and document pipelines |
| ***OCR & Document Parsing*** | PyMuPDF + Tesseract OCR | Extract text from prescriptions, reports, and scanned documents |
| ***Local LLM Server*** | Ollama | Self-hosted model serving and inference |
| ***Chat & Extraction Model*** | Qwen 2.5 3B | Medical information extraction, intent routing, summarization, and RAG responses |
| ***Embedding Model*** | mxbai-embed-large | High-quality semantic embeddings for medical document retrieval |
| ***Relational Database*** | PostgreSQL (Neon) | Structured patient timeline, diagnoses, medications, and metadata |
| ***Vector Database*** | Qdrant Cloud | Semantic search and retrieval over medical memories |
| ***ORM & Database Client*** | Prisma | Type-safe PostgreSQL access and schema management |
| ***Containerization*** | Docker & Docker Compose | Local development and deployment consistency |
| ***API Documentation*** | OpenAPI / Swagger | Interactive API exploration and testing |
