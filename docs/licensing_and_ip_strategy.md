# Legal Intelligence Platform: Licensing & IP Strategy

**Status**: Draft for Enterprise Release | **Date**: February 2026

---

## 1. Core Philosophy: "Client Data is Sacred"

Following the industry standard set by **Harvey AI** and **CoCounsel**, our platform adopts a **Zero-Training Policy** by default.

### The "No-Train" Guarantee
> *"We do NOT use your submitted legal documents, case files, or extraction outputs to train our base foundational models."*

*   **Why**: Law firms cannot risk client confidentiality.
*   **Competitor Benchmark**: Harvey and Ironclad explicitly state this to win Enterprise contracts.
*   **Implementation**: Open-source models (e.g., Llama 3 on-prem) or Zero-Retention APIs (OpenAI Enterprise).

---

## 2. Intellectual Property (IP) Ownership

We distinguish clearly between "The Engine" and "The Fuel".

| Asset Type | Description | Ownership |
| :--- | :--- | :--- |
| **The Platform** | The Source Code, UI, Algorithms, Regex Logic, and Pre-trained Vectors. | **US (The Vendor)** |
| **Customer Data** | Uploaded PDFs, Word Docs, Evidence. | **YOU (The Client)** |
| **Generated Output** | The Extracted Timeline, Metadata, and RAG Answers. | **YOU (The Client)** |
| **Feedback** | User suggestions for feature improvements. | **US (The Vendor)** |

### Usage License
We grant the client a **non-exclusive, non-transferable, revocable license** to use the software strictly for internal legal operations.

---

## 3. Data Security & Compliance Standards

To compete with Ironclad/CoCounsel, we adhere to the following security posture:

1.  **Encryption**: AES-256 for Data-at-Rest (Postgres/S3) and TLS 1.3 for Data-in-Transit.
2.  **Isolation**: Logical tenant separation (Row-Level Security in Postgres).
3.  **SOC 2 Readiness**:
    *   *Audit Logs*: rigorous logging of "Who accessed What Document When".
    *   *Access Control*: Role-Based Access Control (RBAC) (Admin vs. Associate).

---

## 4. Commercial Licensing Models

We propose a **Tiered SaaS Model**:

### A. "Solo Practitioner" License
*   **Cost**: Monthly Subscription.
*   **Limits**: Up to 50 active cases.
*   **Deployment**: Cloud-hosted (Multi-tenant).

### B. "Firm" License (Enterprise)
*   **Cost**: Per-Seat + Usage Volume.
*   **Features**: SSO (Okta), Audit Logs, Priority Support.
*   **Deployment**: Virtual Private Cloud (VPC) isolation option.

### C. "On-Premise" License (Government/Judiciary)
*   **Cost**: Annual Licensing Fee + Maintenance.
*   **Deployment**: Fully air-gapped installation on client servers (Docker).
*   **Control**: 100% Data Sovereignty.

---

## 5. Indemnification & Liability

*   **We Indemnify**: The Client against IP infringement claims regarding *Our Platform*.
*   **Client Indemnifies**: Us against claims regarding *Their Content* (e.g., uploading illegal/stolen documents).
*   **Limitation of Liability**: Capped at 12 months of subscription fees (Industry Standard).
