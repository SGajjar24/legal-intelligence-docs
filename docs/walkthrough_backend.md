# Phase 2: Intelligence System — Running Instructions (FINAL)

#### System Status

✅ **Intelligence-Ready**

**Enabled Capabilities**

* Async OCR & text extraction
* Deterministic legal metadata extraction
* Timeline/Event generation
* RAG-ready embedding storage

---

### How to Run the Full Stack

#### 1️⃣ Prerequisites

Ensure Docker is running and infrastructure is up (required for API/Workers):

```bash
docker-compose up -d
```

#### 2️⃣ (Offline) Logic Verification

If Docker is down, you can still verify the **Intelligence Layer** (OCR + Regex) using the offline scripts.

**Single File Test:**
```bash
python -m tests.logic_verification
```

**Batch Folder Test:**
```bash
python -m tests.batch_logic_verification
```
*   Verifies extraction on all PDFs in `JAYSHREE_GAJJAR...` folder.
*   Prints extracted Doc Types, Dates, and Sections.

---

### How to Run the Live System (Requires Docker)

#### 3️⃣ Start the API (Terminal 1)

```bash
cd Legal_tech
uvicorn app.main:app --reload
```

#### 4️⃣ Start the Worker (Terminal 2)

```bash
cd Legal_tech

# Windows
celery -A app.worker worker --loglevel=info --pool=solo

# Linux / Mac
celery -A app.worker worker --loglevel=info
```

#### 5️⃣ Intelligence Pipeline (Guaranteed Order)

Each document follows a **strict pipeline**:

```
process_document
→ extract_legal_metadata
→ create_embeddings
```

(Order enforced via Celery chain.)

#### 6️⃣ Test the Intelligence Layer

**Upload a document**

```http
POST http://localhost:8000/api/v1/ingest
```

**Observe worker logs**

* `process_document` → text extraction / OCR
* `extract_legal_metadata` → dates, sections, authority
* `create_embeddings` → chunk vectors stored

#### 7️⃣ Verify Database Outputs

**documents table**

* `document_type`
* `authority`
* `stage`
* `sections[]`
* `status = COMPLETED`

**events table**

* `event_date`
* `event_type`
* `authority`
* `confidence_score`

Successful ingestion = **both tables populated correctly**.
