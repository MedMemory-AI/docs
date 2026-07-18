# 📚 API Reference

Welcome to the **MedMemory AI Engine API Reference**.

MedMemory is a production-grade local RAG and clinical analytics engine running on **OpenAPI Specification (OAS 3.1)**.

## Base URL

```http
http://localhost:8000/api/v1
```

For complete interactive schemas, request serialization validation, and one-click API testing, visit the local Swagger UI:

```http
http://localhost:8000/docs
```

---

## 🩺 System Health

### Check Health

```http
GET /health
```

Verifies the operational status of the MedMemory engine.

#### Response — `200 OK`

```json
{
  "status": "ok"
}
```

---

## 🔐 Stateless JWT Authentication

### Register

```http
POST /auth/register
```

Creates a new user account.

#### Request — `RegisterRequest`

```json
{
  "fullName": "John Doe",
  "email": "user@example.com",
  "password": "password123"
}
```

#### Response — `AuthSuccessResponse`

```json
{
  "success": true,
  "message": "Registration successful",
  "data": {
    "id": "usr_94821",
    "fullName": "John Doe",
    "email": "user@example.com",
    "accessToken": "eyJhbGciOi...",
    "tokenType": "Bearer"
  }
}
```

---

### Login

```http
POST /auth/login
```

Authenticates a user and issues a bearer access token.

#### Request — `LoginRequest`

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

#### Response — `AuthSuccessResponse`

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "id": "usr_94821",
    "fullName": "John Doe",
    "email": "user@example.com",
    "accessToken": "eyJhbGciOi...",
    "tokenType": "Bearer"
  }
}
```

---

### Logout

```http
POST /auth/logout
```

Invalidates the active session token.

---

## 📂 Ingestion

### Upload File

```http
POST /ingestion/upload
```

Uploads medical records such as:

- Prescriptions
- Lab Reports
- Scan Reports
- Discharge Summaries

The uploaded document is processed through the OCR extraction, text cleaning, and Named Entity Recognition (NER) pipeline.

#### Request

```http
Content-Type: multipart/form-data
```

**Body:**

```text
file: Binary payload (application/octet-stream)
```

#### Response — `IngestionSuccessResponse / UploadResponse`

```json
{
  "success": true,
  "message": "File ingested successfully",
  "data": {
    "filePath": "/storage/uploads/prescription_01.pdf",
    "mimeType": "application/pdf",
    "size": 1048576,
    "cleaned_text": "Patient exhibits minor hypertension...",
    "ner": {},
    "extraction": {},
    "createdAt": "2026-07-18T11:47:22Z"
  }
}
```

---

## 🗓️ Timeline

### Fetch Patient Timeline

```http
GET /timeline/
```

Returns chronologically sorted historical clinical insights for frontend visualization.

#### Response — `TimelineSuccessResponse`

```json
{
  "success": true,
  "message": "Timeline fetched successfully",
  "patient_id": "usr_94821",
  "count": 1,
  "timeline": [
    {
      "id": "doc_001",
      "patientId": "usr_94821",
      "docType": "Prescription",
      "date": "2026-07-15",
      "doctor": "Dr. Sarah Jenkins",
      "findings": {},
      "diseases": [
        "Hypertension"
      ],
      "highlights": "Start Metoprolol 25mg daily",
      "treatment": "Pharmacotherapy",
      "preDiagnosis": "Essential Hypertension",
      "createdAt": "2026-07-15T09:00:00Z"
    }
  ]
}
```

---

## 🤖 AI Copilot

### Query Copilot

```http
POST /chat/
```

Submits a natural-language query against the patient's medical context and returns a validated, structured RAG-generated answer.

#### Request — `ChatRequest`

```json
{
  "query": "When was diabetes diagnosed?"
}
```

#### Response — `ChatResponse`

```json
{
  "success": true,
  "answer": "Diabetes was diagnosed on March 24, 2025, according to the discharge summary from City Hospital.",
  "sources": [
    {
      "id": "doc_002",
      "docType": "Discharge Summary",
      "date": "2025-03-24",
      "doctor": "Dr. Amit Patel"
    }
  ]
}
```

---

### Query Copilot Stream

```http
POST /chat/stream
```

Provides the same RAG query functionality as `/chat/`, but streams generated tokens using **Server-Sent Events (SSE)** for low-latency, real-time UI rendering.

#### Response Content Type

```http
Content-Type: text/event-stream
```

Typical flow:

```text
User Query
    ↓
Intent Classification
    ↓
Qdrant Semantic Retrieval
    ↓
Optional PostgreSQL Context Retrieval
    ↓
Prompt Construction
    ↓
LLM Generation
    ↓
SSE Token Stream
    ↓
Client UI
```

---

## 🔑 Authentication

Protected endpoints require a JWT bearer token in the `Authorization` header.

```http
Authorization: Bearer <access_token>
```

Example:

```http
Authorization: Bearer eyJhbGciOi...
```

The access token is returned after successful registration or login.

---

## 📌 API Summary

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/health` | Check engine health |
| `POST` | `/api/v1/auth/register` | Register a user |
| `POST` | `/api/v1/auth/login` | Authenticate a user |
| `POST` | `/api/v1/auth/logout` | Logout / invalidate session |
| `POST` | `/api/v1/ingestion/upload` | Upload and process a medical document |
| `GET` | `/api/v1/timeline/` | Retrieve patient medical timeline |
| `POST` | `/api/v1/chat/` | Query patient medical history using RAG |
| `POST` | `/api/v1/chat/stream` | Stream RAG-generated response using SSE |

---

> **Note:** MedMemory processes sensitive medical information. Production deployments should use HTTPS, secure JWT handling, strict patient-level data isolation, appropriate access controls, and applicable healthcare/privacy compliance measures.
