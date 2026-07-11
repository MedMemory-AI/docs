# ⚙️ Core System Workflows

MedMemory AI processes data through two highly specialized engine workflows: an asynchronous ingestion pipeline and a stateful conversational query router.

---

## 1. Document Ingestion Pipeline

This workflow parses multi-format, unstructured clinical records and extracts them into atomic, verifiable entries.

### Execution Sequence:
1.  **Ingestion & OCR:** Raw PDFs, scans, and image prescriptions are parsed using `Docling` to extract clean structural layout text blocks.
2.  **Normalization & Splitting:** Text undergoes noise removal and headers/footers cleaning before being split into meaningful sentence structures via `spaCy`.
3.  **Medical NER:** `scispacy` and `MedCAT` analyze chunks to tag critical medical entities (diseases, symptoms, dosages, specific drugs).
4.  **Structured Event Construction:** A local LLM takes the tagged sentences and builds exact chronological events matching explicit Pydantic JSON validation schemas.
5.  **Hybrid Storage Layer:**
    * **PostgreSQL:** Stores rigid chronological event rows (Patient Profile, Event Timestamps, Medications, Entities, Labs).
    * **Qdrant Vector DB:** The LLM generates summary text strings, which are compiled into 1024-dimension vectors via `mxbai-embed-large` and indexed for semantic searches.

![Ingestion-Workflow](https://github.com/MedMemory-AI/docs/tree/main/assets/workflow-ingestion.png)

---

## 2. Chatbot Pipeline

This workflow utilizes a cyclic multi-agent graph to contextually navigate a patient's historical records during conversational turns.

### Execution Sequence:
1.  **Intent Classification:** The user submits a prompt (e.g., *"What is its dose?"*). A local LLM intent classifier evaluates the request along with active conversation history to detect information targets.
2.  **Parallel Data Retrieval Router:**
    * **SQL Path (PostgreSQL):** Resolves structured queries tracking relational data points (timeline sorting, specific labs, exact numerical metrics).
    * **Vector Path (Qdrant):** Converts questions into semantic text vectors to retrieve matches across narrative notes and doctor summaries.
3.  **Context Builder & Synthesis:** Retrieved tables and embedding fragments are combined, deduplicated, and ranked into a clean context package.
4.  **Guardrailed Generation:** The compiled context passes to a localized `Ollama / llama3.2` engine instructed strictly to form answers based *only* on the injected data to completely eliminate hallucinations.
5.  **Memory Synchronization:** The conversational state and final response loop back into LangGraph's persistent history layer, allowing the engine to cleanly handle follow-up context in the next turn.

![Chatbot-Workflow](https://github.com/MedMemory-AI/docs/tree/main/assets/workflow-chatbot.png)
