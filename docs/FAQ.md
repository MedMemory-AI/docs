# ❓ Frequently Asked Questions

---

## What is MedMemory AI?

MedMemory AI is an AI-powered personal medical memory system that transforms medical documents into a searchable health timeline and conversational medical history.

---

## Is MedMemory an Electronic Health Record (EHR)?

No.

MedMemory focuses on building an AI-powered medical memory layer rather than replacing traditional EHR systems.

---

## What documents can be uploaded?

Currently:

- Prescriptions
- Lab Reports
- Scan Reports
- Discharge Summaries

---

## Does MedMemory store uploaded PDFs?

The platform primarily stores extracted structured medical information and semantic memory representations.

Storage strategies may evolve as the project matures.

---

## Which AI models are used?

By default:

```text
qwen2.5:3b
mxbai-embed-large
```

through Ollama.

---

## Which databases are used?

### PostgreSQL

Stores:

- Patient records
- Medical documents
- Timeline information

### Qdrant

Stores:

- Embeddings
- Semantic memory

---

## Can I self-host MedMemory?

Yes.

The project is designed with self-hosting in mind.

Deployment guides are available in the documentation.

---

## Does MedMemory require internet access?

For local deployments using Ollama:

- Document processing can run locally.
- Chat functionality can run locally.

Cloud deployments may require internet access for managed services.

---

## Is MedMemory production ready?

Currently the project is under active development and should be considered experimental.

---

## How can I contribute?

See:

```text
Guides → Contributing Guide
```

for contribution instructions.

---

## Where can I report bugs?

Open an issue on GitHub:

https://github.com/MedMemory-AI/MedMemory/issues
