# Phase 3: Frontend Visualization — Running Instructions

#### System Status

✅ **Frontend Initialized**
✅ **Dashboard Implemented**

The "Lawyer's Cockpit" is now ready for initial review.

---

### How to Run the Frontend

#### 1️⃣ Start the Development Server

Navigate to the frontend directory:

```bash
cd legal_tech_frontend
```

Run the dev server:

```bash
npm run dev
```

#### 2️⃣ Access the Application

Open your browser to:

👉 **[http://localhost:3000](http://localhost:3000)**

You will be automatically redirected to the **Dashboard**, where you can see:
*   **Key Statistics** (Active Cases, Documents)
*   **Recent Batches** (Mock Data for UI review)
*   **Sidebar Navigation**

---

### Next Steps (Implementation)

The current UI uses **mock data**. The next tasks will connect this to the real Phase 2 backend:

1.  **Connect API**: Replace mock data with `useQuery` hooks fetching from FastAPI.
2.  **Timeline View**: Implement the visual event graph.
3.  **Document Viewer**: Build the split-screen reading experience.
