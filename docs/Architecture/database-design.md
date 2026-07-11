# 🗄️ Database & Vector Store Schema Design

MedMemory AI splits data across a transactional relational layer (PostgreSQL) and a dense semantic retrieval layer (Qdrant Vector DB) to ensure fast chronological tracking alongside high-accuracy RAG capabilities.

---

## 🐘 1. Relational Database Layer (PostgreSQL)

This schema tracks rigid structural parameters, clinical entities, timelines, and relationships. 

### `patients` Table
*Primary profile storage for local sandbox tracking.*

| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` | `PRIMARY KEY` | Unique identification token for the patient. |
| `full_name` | `VARCHAR(255)` | `NOT NULL` | The legal full name of the patient. |
| `email` | `VARCHAR(255)` | `UNIQUE`, `NOT NULL` | Electronic mailing login reference address. |
| `password` | `VARCHAR(255)` | `NOT NULL` | Encrypted password hash string (bcrypt/argon2). |

### `prescriptions` Table
*Captures transactional details extracted directly from doctor prescriptions.*

| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` | `PRIMARY KEY` | Unique prescription entry identification key. |
| `patient_id` | `UUID` | `FOREIGN KEY` | References `patients(id)`. |
| `date` | `DATE` | `NOT NULL` | The official date the prescription was written by the doctor. |
| `highlights` | `TEXT` | `NULLABLE` | Symptoms, observations, or chief complaints noted by the clinician. |
| `treatment` | `TEXT` | `NULLABLE` | Structured lists of medications, dosages, frequencies, or therapies. |
| `pre_diagnosis` | `TEXT` | `NULLABLE` | Ordered diagnostics (e.g., specific blood tests, MRI scans). |
| `doctor` | `VARCHAR(255)` | `NULLABLE` | Name or identifier of the prescribing medical professional. |

### `reports` Table
*Stores factual findings extracted from unstructured lab reports or diagnostic scans.*

| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `UUID` | `PRIMARY KEY` | Unique report document identification key. |
| `patient_id` | `UUID` | `FOREIGN KEY` | References `patients(id)`. |
| `date` | `DATE` | `NOT NULL` | The execution or collection timestamp of the lab work. |
| `findings` | `JSONB` / `TEXT` | `NOT NULL` | Exact raw numerical values or text metrics (e.g., RBC count: 4.8). |
| `diseases` | `TEXT[]` | `NULLABLE` | Array of confirmed clinical conditions detected (e.g., `["Kidney Stones"]`). |
| `doctor` | `VARCHAR(255)` | `NULLABLE` | Reviewing or authorizing laboratory clinician. |

---

## 🌀 2. Dense Semantic Vector Layer (Qdrant)

The Qdrant index tracks unstructured contextual semantics. The payload fields are split into searchable content arrays and strict relational attributes used exclusively as high-speed query execution filters.

* **Collection Name:** `clinical_memories`
* **Vector Signature:** 1024 Dimensions (`mxbai-embed-large`)
* **Distance Metric:** Cosine Similarity

### Payload & Metadata Architecture

| Field Component | JSON Key Name | Data Type | Search Configuration | Description / Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Vector Input** | `page_content` | `TEXT` | **Embedded (Vectorized)** | LLM-generated concise clinical summaries combining `highlights`, `treatment`, `pre_diagnosis`, `findings`, and `diseases`. Used for dense semantic vector RAG search queries. |
| **Metadata Filter**| `date` | `STRING` (ISO) | **Indexed (No Embedding)** | The exact calendar date of the clinical event. Used for chronological time-window pre-filtering. |
| **Metadata Filter**| `doctor` | `STRING` | **Indexed (No Embedding)** | Name of the treating clinician. Used to isolate records from specific provider names. |
| **Metadata Filter**| `patient_id` | `STRING` | **Indexed (No Embedding)** | Relational anchor string. Ensures absolute multi-tenant privacy separation inside the shared vector collection. |
| **Metadata Filter**| `source_type` | `STRING` | **Indexed (No Embedding)** | Explicit tracking tag indicating parent source provenance (`"prescription"` vs. `"report"`). |
