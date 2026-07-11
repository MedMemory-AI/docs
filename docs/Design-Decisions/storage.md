# 🗄️ Storage Engine Choices: Why Postgres + Qdrant?

We rejected a pure-vector database architecture in favor of a hybrid system:

### 1. Relational Chronology (PostgreSQL)
Medical data is inherently tied to structured timelines (e.g., specific test dates, drug dosages). Relational structures allow us to run precise transactional queries:
*   Enforcing strict foreign-key lookups between patients, treatments, and appointments.
*   Providing fast chronological sorting for historical timelines.

### 2. Semantic Intent Search (Qdrant)
Clinical narratives contain variations in nomenclature (e.g., "high blood pressure" vs. "hypertension"). 
*   **Qdrant** allows us to store 1024-dimension vectors efficiently.
*   It handles lightning-fast semantic querying over dense textual data, running side-by-side with Postgres to enrich structural context.
