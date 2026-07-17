# 📚 API Reference

This page documents the primary MedMemory API endpoints.

Base URL:

```text
http://localhost:8000
```

---

# Authentication

## Register

```http
POST /auth/register
```

### Request

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

---

## Login

```http
POST /auth/login
```

### Response

```json
{
  "access_token": "...",
  "token_type": "bearer"
}
```

---

# Documents

## Upload Document

```http
POST /documents/upload
```

Uploads:

- Prescription
- Lab Report
- Scan Report
- Discharge Summary

---

## List Documents

```http
GET /documents
```

Returns all uploaded medical documents.

---

# Timeline

## Get Timeline

```http
GET /timeline
```

Returns chronological medical history.

---

# Chat

## Chat Query

```http
POST /chat
```

### Request

```json
{
  "query": "When was diabetes diagnosed?"
}
```

### Response

```json
{
  "answer": "Diabetes was diagnosed on..."
}
```

---

# Health Check

## Health

```http
GET /health
```

### Response

```json
{
  "status": "ok"
}
```

---

For complete request and response schemas, refer to the Swagger documentation.

```text
http://localhost:8000/docs
```