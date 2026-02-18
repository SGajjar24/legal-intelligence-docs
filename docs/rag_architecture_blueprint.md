# Legal Tech RAG Blueprint & Architecture

## 1. Stakeholders & Needs Analysis

| Stakeholder | Primary Need | Use Case Example | RAG Requirement |
| :--- | :--- | :--- | :--- |
| **Lead Counsel** | **Strategic Insight** | "Find all precedents where Section 148A was quashed due to procedural delay." | **High Accuracy**: Must retrieve exact case numbers and dates. No hallucinations allowed. |
| **Junior Associate** | **Review & Drafting** | "Draft a reply to this notice citing our last 3 successful appeals." | **Context Awareness**: Needs to access both the *Current Notice* and *Past Appeals*. |
| **Clerk / Admin** | **Organization** | "File this notice in the correct case folder." (Auto-classification) | **Classification**: Needs to "read" the document type and match it to a Case ID. |
| **Client** (Future) | **Status Updates** | "What is the next hearing date for my case?" | **Simplicity**: Needs a chatbot interface that queries the specific case file safely. |

---

## 2. RAG System Architecture

The system is built on a **"Retrieve-Then-Reason"** pattern.

### High-Level Data Flow
1.  **Ingestion**: User uploads PDF -> System OCRs it.
2.  **Indexing**: System splits text into "Chunks" -> Creates Vectors -> Stores in DB.
3.  **Retrieval**: User asks question -> System searches Vectors -> Finds top 5 chunks.
4.  **Generation**: System sends Question + Top 5 Chunks to LLM -> LLM writes answer.

### System Diagram (Mermaid)

```mermaid
graph TD
    %% Ingestion Pipeline
    subgraph "Ingestion Pipeline (Async)"
        U[User Upload] -->|PDF/Image| Q[Redis Queue]
        Q --> W[Python Worker (Celery)]
        W -->|1. Extract| OCR[Tesseract / PyMuPDF]
        OCR -->|2. Chunk| Splitter[Text Splitter]
        Splitter -->|3. Embed| Model[Embedding Model]
        Model -->|Vectors| DB[(Postgres + pgvector)]
        OCR -->|Raw Text| DB
        W -->|File| S3[Object Storage]
    end

    %% Query Pipeline
    subgraph "Query Engine (FastAPI)"
        User[User Query] -->|Ask Question| API[FastAPI Backend]
        API -->|Embed Query| Model
        API -->|Vector Search| DB
        DB -->|Top 5 Contexts| API
        API -->|Prompt + Context| LLM[Google Gemini / GPT-4]
        LLM -->|Answer with Citations| User
    end
```

---

## 3. Technical Requirements

### A. The "Brain" (LLM & Embeddings)
*   **LLM**: **Google Gemini 1.5 Flash** (Fast, huge context window) or **GPT-4o mini** (Cheap, smart).
    *   *Why*: Legal documents are long. Gemini's 1M+ token context window is a game-changer for reading entire case files at once.
*   **Embeddings**: **OpenAI `text-embedding-3-small`** or **HuggingFace `all-MiniLM-L6-v2`** (Self-hosted for privacy).
    *   *Recommendation*: `text-embedding-3-small` (Balance of cost/performance/dimension size).

### B. The "Memory" (Vector Database)
*   **Database**: **PostgreSQL** with `pgvector` extension.
    *   *Why*: You already need Postgres for relational data (Client Names, Dates). Keeping Vectors in the same DB simplifies backup and queries ("Find vectors WHERE case_id = 123").
    *   *Alternative*: Pinecone or Weaviate (Add complexity and cost; stick to Postgres first).

### C. The "Reader" (OCR & Parsing)
*   **Engine**: **Tesseract 5** (You effectively own this workflow already) + **PyMuPDF**.
*   **Chunking Strategy**:
    *   **RecursiveCharacterTextSplitter**: Standard approach.
    *   *Legal Twist*: Split by **"Header"** or **"Paragraph Number"**. Legal documents are structured. We should try to keep "Point 1" and "Point 2" in separate chunks.

---

## 4. Implementation Steps (RAG)

1.  **Database Setup**:
    *   Install PostgreSQL.
    *   Enable extension: `CREATE EXTENSION vector;`
    *   Create `document_chunks` table with a `vector(1536)` column.

2.  **Ingestion Script Refactor**:
    *   Modify `deep_organizer.py` or create a new `ingest_to_db.py`.
    *   Instead of just moving files, it must now:
        *   Read Text.
        *   Call Embedding API.
        *   Insert Rows into DB.

3.  **The "Chat" Endpoint**:
    *   Create a simple script `chat_with_case.py`.
    *   Input: `case_id`, `question`.
    *   Logic: SQL Query (`SELECT text FROM chunks ORDER BY embedding <=> query_embedding LIMIT 5`) -> Construct Prompt -> Call LLM.

This blueprint gives you a sophisticated, professional-grade Legal AI backbone without over-engineering.
