<div align="center">

# Hi, I'm Abdullah Gamil 👋

### AI Engineer — Computer Vision · Agentic Systems · LLMs & RAG

Building production-grade AI systems and pairing them with the software engineering discipline to make them trustworthy.

📍 Saudi Arabia &nbsp;·&nbsp; 🎓 Computer Engineering, Çanakkale Onsekiz Mart University

[![Email](https://img.shields.io/badge/Email-ab.aljameel%40hotmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:ab.aljameel@hotmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Abdullah--Gamil-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/Abdullah-gamil)
[![GitHub](https://img.shields.io/badge/GitHub-ab--jameel-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/ab-jameel)

</div>

---

## About Me

I'm an AI Engineer who works across the full stack of a model's life — from training a computer vision pipeline to shipping the RBAC-secured web app that puts it in front of a user. My focus is **agentic AI and production ML systems**: retrieval-augmented pipelines, LangGraph-based agents, and computer vision models that hold up outside a notebook.

What I care most about is the gap between "the model works" and "the system is safe to run." That shows up in how I build: locked holdout sets that never touch training, human-authorized write-backs instead of confidence-based auto-actions, and test suites written to catch the bug *before* a reviewer does. A few fast facts:

- 🏥 Trained a **YOLOv8-seg** model on 21k+ dental X-rays for an SaaS diagnostic tool, catching a training/validation leakage bug that had inflated in-training metrics 2x
- 🤖 Built a **LangGraph financial reconciliation agent** and documented all 27 real bugs found in evaluation — because "confidence must never be authorization" needs to survive contact with real data
- 🩻 Reached **91% test accuracy** on chest X-ray pneumonia classification with a transfer-learned ResNet-50
- 🧪 Shipped a Flask RBAC system backed by a **234-test** automated suite, catching a critical cross-tenant scope-leakage bug before delivery

---

## 🧰 Tech Stack

**AI / ML**
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)

**Backend / Full-Stack**
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

**Data / Infra**
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

---

## 🚀 Featured Projects

### 🧾 Financial Reconciliation Agent — LangGraph
An autonomous agent that reconciles bank statements against ledger entries, using a deterministic matcher for clean cases and a ReAct-style LLM investigator (with policy retrieval over a Qdrant vector store) for exceptions. The core design rule: **no confidence score, model or human, is ever itself an authorization to post** — only a signed human decision is.

Built on FastAPI + Postgres, with PDF statement ingestion, an idempotent write-back layer to a mock ERP, and an evaluation harness run against a held-out synthetic dataset.

<details>
<summary><b>Why this project is the strongest signal of how I debug (click to expand)</b></summary>
<br>

I kept a build log of all 27 real bugs and design gaps found — not a cleaned-up postmortem, but the actual symptom → root cause → fix trail as it happened:

- **Caught a policy violation, not just a bug**: evaluation showed `duplicate` and `missing_invoice` cases silently auto-clearing, directly contradicting the project's own written policy that duplicates must never auto-clear "regardless of confidence score." Fixed with an invoice-existence check and an atomic Postgres claim so two transactions can't win the same ledger entry.
- **A parsing bug, not a prompt bug**: the investigator was scoring 100% "policy_sensitive" despite correct underlying reasoning — traced to `json.loads()` rejecting valid JSON with reasoning text in front of it, not to weak prompting. Fixing the parser (not the prompt) moved `correct_disposition_rate` from 0.182 to 0.591 in one change.

Full write-up (symptom, root cause, fix, and why it matters for every item) is in the repo.
</details>

**Stack:** LangGraph · FastAPI · PostgreSQL · Qdrant · litellm · pytest
[Repo →](https://github.com/ab-jameel/Financial-Reconciliation-Agent)

---

### 🦷 DentAI Pro — Dental Disease Detection SaaS
A diagnostic-aid platform that flags disease on panoramic dental X-rays for dentists (not a replacement for one). Trained a **YOLOv8-seg** model across 21k+ images merged from two independently-licensed datasets, using a two-stage pretrain → fine-tune pipeline with a permanently locked, never-trained-on holdout set as the only trustworthy evaluation.

- Caught a train/validation leakage bug that had inflated in-training metrics roughly 2x (0.993 in-training vs. 0.537 on the real holdout)
- Shipped 3 of 4 target pathology classes after holdout evaluation showed the 4th (Bone Loss) recalled under 10% of real cases and had no way to be independently verified — excluded rather than shipped
- Vetted every candidate dataset for license safety and taxonomy overlap before training, excluding one dataset entirely after determining it was a duplicate export of another with identical class fingerprints

**Stack:** YOLOv8-seg · FastAPI · PostgreSQL + MinIO · React + Cornerstone.js · AWS · Docker
[Repo →](https://github.com/ab-jameel/Dentai-pro)

---

### 💰 Finance & Grade Management Platforms
Two enterprise-style, role-based systems built end-to-end for real organizations:

| | Finance Reports Creator | Grade Management System |
|---|---|---|
| **For** | A nonprofit foundation (Al-Madinah Development Foundation) | A multi-branch educational program |
| **Scope** | 4 departments (Finance, Outreach, Education, Admin) | Country → Branch → Program → Intake hierarchy |
| **Access control** | 38 granular permission flags | 4-tier role hierarchy with scoped data access |
| **Highlights** | Multi-currency ledgers, multi-stage approval workflows, 11 automated Arabic PDF report types (TCPDF) | 234-test pytest suite; caught and fixed a critical cross-branch scope-leakage bug before delivery |
| **Stack** | PHP · MySQL · TCPDF · Arabic RTL UI | Flask · SQLAlchemy · Flask-Login · Bootstrap 5 RTL |

---

### 🩻 Chest X-Ray Pneumonia Classifier
A ResNet-50 transfer-learning model classifying chest X-rays as Normal or Pneumonia, with augmentation, learning-rate scheduling, and early stopping against a validation set.

**Result:** 90.5% test accuracy, 0.91 weighted F1 across both classes on a 624-image held-out test set.

**Stack:** TensorFlow/Keras · ResNet50 · OpenCV · scikit-learn
[Repo →](https://github.com/ab-jameel/Chest-X_ray-Resnet)

---

### 📚 RAG Study Assistant
A local, fully offline study tool: upload PDFs/PPTX/TXT, ask questions, get answers grounded in the source material with page-level citations — no API keys, no data leaving the machine.

**Stack:** Streamlit · LangChain · Chroma · Ollama (`qwen2.5` + `nomic-embed-text`)
[Repo →](https://github.com/ab-jameel/Study_Assistant)

---

### 💬 WhatsApp Clinic Bot
A patient-facing WhatsApp bot for a dental & dermatology clinic — booking, rescheduling, cancellations, and newsletter subscriptions via the Meta Cloud API — plus staff-only commands for broadcasts and conversation follow-up handling.

**Highlights:** webhook signature verification, rate limiting, corrupt-file auto-backup and recovery, safe atomic file writes for a no-database JSON persistence layer.

**Stack:** Node.js · Express · Meta WhatsApp Cloud API

---

## 📂 Other Projects

| Project | Description | Stack |
|---|---|---|
| **University Management System (UBYS)** | Role-based portals (student/teacher/admin) for course registration, grading, and content delivery, fully localized in Turkish | PHP · MySQL |
| **Camasirhane (Dorm Laundry System)** | Self-service laundry booking with cooldown/capacity rules, admin reporting, and PDF/Excel export | Django · SQLite |
| **Diabetes Classification** | KNN classifier on the Pima Indians dataset with full EDA and hyperparameter search | scikit-learn · pandas · seaborn |

---

## 💼 Experience

**AI & Full-Stack Software Engineering** — Freelance · 2021–2026
Delivered production systems end-to-end for real clients: the WhatsApp clinic bot, the Flask grade-management platform, and the multi-department nonprofit finance platform described above.

**AI & Full-Stack Engineering Intern** — Scramblebit · 2025
Built a semantic search engine using vector embeddings for the MarketFlow e-commerce system to improve product discoverability, and a sentiment-analysis pipeline over customer reviews.

---

## 🎓 Education & Certifications

**B.Sc. Computer Engineering** — Çanakkale Onsekiz Mart University (2021–2025)

Hugging Face Agents Course · AWS Educate Machine Learning Foundations · CodeSignal Deploying ML Models · Deep Learning Guide (Udemy) · Full Stack Development (Udacity)

---

## 📊 GitHub Stats

<div align="center">

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=ab-jameel&show_icons=true&theme=default&hide_border=true)
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=ab-jameel&layout=compact&hide_border=true)

</div>
