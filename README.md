diff --git a/README.md b/README.md
index e69de29..0c1f2b5 100644
--- a/README.md
+++ b/README.md
@@
-# SkillConnect
-
-
-SkillConnect is a decentralized marketplace for skilled workers and households, featuring live GPS tracking and escrow payments.
-
-## Production Deployment Notes
-
-### Backend on Render (Free Tier)
-
-This project is currently deployed on Render's Free Tier. 
-
-**Important Note on Cold Starts:**
-
-Render automatically spins down free web services after 15 minutes of inactivity. When you visit the app after a period of inactivity, the very first API request or Socket.io connection may take **30-50 seconds** to respond. 
-
-*If the app appears frozen or stuck on a loading spinner during initial visit, please wait a minute! This is a known Render limitation, not a bug in the code.*
-
-**Recommendations:**
-
-1. Upgrade to a paid \"Starter\" instance on Render ($7/mo) to prevent sleeping entirely.
-2. Alternatively, set up a free uptime monitor (like [UptimeRobot](https://uptimerobot.com)) to ping `https://skillconnect-backend-97u2.onrender.com/api/workers/nearby` every 10 minutes to keep the instance alive 24/7.
+# 🚀 SkillConnect
+
+> **A decentralized marketplace that connects skilled workers with households, offering live GPS tracking, secure escrow payments, and AI‑enhanced matching.**
+
+![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
+![FastAPI](https://img.shields.io/badge/FastAPI-Backend-green)
+![React](https://img.shields.io/badge/React-Frontend-blue)
+![Supabase](https://img.shields.io/badge/Supabase-Auth%2FDB-3ECF8E)
+
+---
+
+# 📖 Overview
+
+Recruiters and homeowners often struggle to find trustworthy, qualified service providers quickly. Traditional platforms rely on manual vetting and simple keyword matching, which can lead to mismatched expectations, security concerns, and delayed payments.
+
+**SkillConnect** solves these problems by providing:
+- Real‑time geolocation of nearby workers.
+- Automated escrow handling to protect both parties.
+- AI‑driven skill matching and recommendation.
+- Secure authentication and data storage via Supabase.
+
+The platform is built as a modern full‑stack web app with a **FastAPI** backend, **React** (or Streamlit if you prefer) frontend, and **Supabase** for auth & PostgreSQL. It showcases a clean micro‑service architecture that can be deployed on Render, Vercel, or any cloud provider.
+
+---
+
+# ✨ Features
+
+* 🔐 **Secure authentication** with Supabase (email/password, magic link, OAuth). 
+* 📍 **Live GPS tracking** – workers broadcast their location; households see them on an interactive map. 
+* 💰 **Escrow payments** – funds are held securely until the job is marked complete. 
+* 🤖 **AI‑powered matching** – skill extraction and semantic similarity rank workers against job requirements. 
+* 📊 **Analytics dashboard** – real‑time stats on jobs, earnings, and SOS alerts. 
+* 🆘 **SOS panic button** – instant emergency alerts to admins with live location. 
+* 📱 **Responsive UI** – works on web browsers and mobile devices (via Capacitor). 
+* 🛠️ **Admin panel** – manage users, monitor SOS incidents, review payments, and generate CSV reports. 
+
+---
+
+# 🛠️ Tech Stack
+
+### Frontend
+* **React** (Create‑React‑App) – UI components, React‑Router, Redux Toolkit.
+* **Leaflet** – map rendering and 2‑dsphere queries.
+* **Socket.io‑client** – real‑time updates (location, SOS, booking status).
+
+### Backend
+* **FastAPI** – high‑performance REST API.
+* **Uvicorn** – ASGI server.
+* **MongoDB** – flexible storage for users, workers, bookings, escrow, SOS logs.
+* **Socket.io** – bi‑directional events for live tracking and chat.
+* **Supabase** – authentication (via JWT), optional DB sync.
+
+### AI & NLP
+* **spaCy** – skill and entity extraction from job descriptions.
+* **Sentence‑Transformers** – generate sentence embeddings for semantic similarity.
+* **Groq LLM** – generate improvement suggestions for workers (optional).
+
+### DevOps
+* **Render (Free tier)** – backend hosting (auto‑scale, HTTPS). 
+* **GitHub Actions** – CI linting & test runs.
+* **UptimeRobot** – keep free Render service alive (ping endpoint).
+
+---
+
+# 🏗️ System Architecture
+
+```text
+                        User
+                          │
+                          ▼
+                 React Frontend (Web / Mobile)
+                          │
+                          ▼
+                   FastAPI Backend (Uvicorn)
+                          │
+        ┌─────────────────┼─────────────────┐
+        ▼                 ▼                 ▼
+ Resume Parser      NLP Processing      Supabase
+        │                 │         Authentication & DB
+        ▼                 ▼
+Sentence Embeddings   Skill Extraction
+        │                 │
+        └──────────┬──────┘
+                   ▼
+          Semantic Similarity Engine
+                   │
+                   ▼
+            Groq LLM (optional)
+                   │
+                   ▼
+          Matching Score & Suggestions
+                   │
+                   ▼
+          Interactive Dashboard (React)
+```
+
+---
+
+# ⚙️ Workflow
+
+1. **Sign‑up / Sign‑in** – Supabase handles JWT issuance.
+2. **Upload / Set profile** – Workers provide skills, hourly rate, and enable GPS.
+3. **Household posts a job** – Title, description, budget, location.
+4. **Backend extracts text**, runs **spaCy** to pull out required skills.
+5. **Sentence‑Transformer** encodes both job description & each worker’s profile.
+6. **Semantic similarity** scores each worker; top‑N are returned.
+7. **Escrow creation** – household pays into escrow; funds are held.
+8. **Live tracking** – worker’s device streams location to backend → Socket.io → household map.
+9. **Job acceptance** – worker accepts, escrow is locked.
+10. **Completion** – worker marks job done → escrow released to worker.
+11. **SOS** – either party can press panic button; admin receives real‑time alert.
+12. **Analytics** – admin dashboards display bookings, earnings, SOS incidents.
+
+---
+
+# 📂 Project Structure
+
+```text
+skillconnect/
+│
+├─ backend/
+│   ├─ api/                # FastAPI routers (auth, workers, bookings, admin)
+│   ├─ models/             # Pydantic models & Mongo schemas
+│   ├─ services/           # Business logic (matching, escrow, SOS)
+│   ├─ utils/              # Helper functions (pdf parsing, embeddings)
+│   └─ main.py             # Application entry point
+│
+├─ frontend/
+│   ├─ src/
+│   │   ├─ components/     # Reusable UI components
+│   │   ├─ pages/          # Route pages (Login, Dashboard, JobBoard, Admin)
+│   │   ├─ store/          # Redux Toolkit slices
+│   │   └─ App.js
+│   └─ public/             # Static assets
+│
+├─ .env.example           # Example env file
+├─ requirements.txt       # Python dependencies
+├─ package.json           # Node dependencies (React)
+├─ README.md              # **This file**
+└─ .gitignore
+```
+
+---
+
+# 🚀 Installation
+
+## 1️⃣ Clone the repository
+
+```bash
+git clone https://github.com/ShivamBharti-29/skillconnect.git
+cd skillconnect
+```
+
+## 2️⃣ Backend setup
+
+### Windows
+
+```bash
+python -m venv .venv
+.venv\\Scripts\\activate
+pip install -r requirements.txt
+```
+
+### Linux / macOS
+
+```bash
+python3 -m venv .venv
+source .venv/bin/activate
+pip install -r requirements.txt
+```
+
+## 3️⃣ Frontend setup
+
+```bash
+cd frontend
+npm install   # or yarn install
+```
+
+---
+
+# 🔑 Environment Variables
+
+Create a `.env` file in the project root (copy from `.env.example`):
+
+```env
+# Backend
+MONGO_URI=mongodb://127.0.0.1:27017/skillconnect
+JWT_SECRET=your_random_secret_key
+GROQ_API_KEY=your_groq_key   # optional, for LLM suggestions
+
+# Supabase
+SUPABASE_URL=https://YOUR-PROJECT.supabase.co
+SUPABASE_ANON_KEY=your_anon_key
+SUPABASE_SERVICE_KEY=your_service_role_key
+```
+
+---
+
+# ▶️ Running the Application
+
+## Backend (FastAPI)
+
+```bash
+uvicorn backend.main:app --reload
+```
+
+Backend URL: `http://127.0.0.1:8000`
+
+## Frontend (React)
+
+```bash
+cd frontend
+npm start   # runs on http://localhost:3000
+```
+
+---
+
+# 🧠 AI Matching Pipeline
+
+```text
+Job Description (text)   →   spaCy skill extraction   →   Sentence‑Transformer embedding
+Worker profiles (text)   →   Same pipeline
+
+Compute cosine similarity → Rank workers → Return top‑N matches
+
+(Optional) Groq LLM → Generate personalized improvement tips for workers.
+```
+
+---
+
+# 📸 Screenshots
+
+*(Add screenshots of the following pages after you run the app)*
+- Login / Sign‑up
+- Household job posting
+- Worker profile & location map
+- Booking flow
+- Admin SOS dashboard
+- Escrow status view
+
+---
+
+# 🎥 Demo
+
+**Live Frontend** – https://skillconnect-frontend.example.com (replace with actual URL when deployed)
+
+**Backend API Docs** – `http://127.0.0.1:8000/docs`
+
+---
+
+# 🚀 Deployment Notes
+
+### Backend (Render, Free Tier)
+- Deployed at `https://skillconnect-backend.onrender.com`.
+- **Cold start**: First request after 15 min of inactivity may take 30‑50 s. Use an uptime monitor (UptimeRobot) to ping `/api/health` every 5 min.
+
+### Frontend (Vercel / Netlify)
+- Connect the `frontend` folder to Vercel for automatic builds.
+- Set environment variables in the Vercel dashboard to match your Supabase project.
+
+---
+
+# 👨‍💻 Author
+
+**Shivam Bharti** – Full‑stack developer
+- GitHub: https://github.com/ShivamBharti-29
+- LinkedIn: https://www.linkedin.com/in/shivam-kumar-bharti/
+
+--- 
+
+*Feel free to open issues or submit pull requests to improve the project!*
