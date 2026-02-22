# 🛡️ Social Media & Abuse Detection System

<div align="center">

![Version](https://img.shields.io/badge/version-2.0.0-6366f1?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![React](https://img.shields.io/badge/React-18+-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![License](https://img.shields.io/badge/license-MIT-10b981?style=for-the-badge)

**Real-time AI-powered content moderation for social media platforms.**  
Detects threats, hate speech, insults, obscenity & sarcasm with contextual intelligence.

[Features](#-features) • [Architecture](#-architecture) • [Setup](#-setup) • [API Reference](#-api-reference) • [Demo](#-demo)

</div>

---

## 🚀 Features

| Feature | Description |
|---|---|
| 🔴 **Real-time Analysis** | Debounced live inference as you type — results in <100ms |
| 📂 **File Upload** | Batch analyze `.txt`, `.csv`, `.json` files (up to 500 rows, parallel inference) |
| 🌐 **URL Analyzer** | Fetch & scan any public web page for toxic content |
| 📊 **Visual Charts** | Area chart (risk history), radar chart (category distribution) |
| ⚙️ **Sensitivity Control** | Adjustable detection threshold (High / Balanced / Strict) |
| 🔴 **Live Stream Sim** | Simulated real-time social media chat moderation |
| 📋 **Session Log** | Full exportable CSV log of all analyzed messages |
| 🎯 **Word Highlighting** | Token-level attention mapping — highlights toxic words |
| 🌙 **Dark Mode UI** | Glassmorphism design with neon glows and smooth animations |

---

## 🧠 Detection Model

The model uses a **multi-layer heuristic NLP pipeline**:

### 5 Toxicity Categories
- ⚠️ **Threat** — Violence, assault, murder, kidnapping, doxxing
- 🚫 **Hate Speech** — Racism, homophobia, antisemitism, supremacism
- 💢 **Insult** — Personal attacks, body shaming, intelligence attacks
- 🤬 **Obscenity** — Profanity, explicit language
- 😏 **Sarcasm** — Passive-aggressive/ironic content

### Detection Layers
```
Layer 1 → Keyword matching        (250+ toxic keywords, scored by category)
Layer 2 → Proximity detection     (derogatory word near person noun → amplify)
Layer 3 → Multi-word phrases      (60+ exact toxic phrase matches)
Layer 4 → Sarcasm patterns        (contextual sarcasm keywords)
Layer 5 → Negation dampening      (not/never/don't before toxic word → reduce score)
Layer 6 → Context modifiers       (gaming/medical/news context → reduce by 55%)
Layer 7 → Safe word whitelist     (80+ safe words never flagged)
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    FRONTEND (React + Vite)               │
│  ┌──────────────┐ ┌────────────┐ ┌──────────────────┐   │
│  │  Text Input  │ │File Upload │ │   URL Analyzer   │   │
│  └──────┬───────┘ └─────┬──────┘ └────────┬─────────┘   │
│         └───────────────┴─────────────────┘             │
│                         │ HTTP / REST API               │
└─────────────────────────┼───────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────┐
│                  BACKEND (FastAPI + Python)               │
│                                                          │
│   POST /analyze        → Single text analysis            │
│   POST /analyze-bulk   → Parallel batch (asyncio.gather) │
│   POST /analyze-file   → Upload + parse txt/csv/json     │
│   POST /analyze-url    → Fetch web page + strip HTML     │
│   GET  /health         → Status check                    │
│                                                          │
│   ┌─────────────────────────────────────────────────┐   │
│   │              ToxicityModel v2                    │   │
│   │  Keywords | Phrases | Proximity | Context-Aware  │   │
│   └─────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────┘
```

---

## 📦 Tech Stack

### Backend
- **FastAPI** — High-performance async REST API
- **Python 3.10+** — Async/await native
- **httpx** — Async HTTP client for URL fetching
- **Uvicorn** — ASGI server with hot-reload

### Frontend
- **React 18** + **Vite** — Fast modern SPA
- **Tailwind CSS v4** — Utility-first styling
- **Framer Motion** — Smooth animations
- **Recharts** — Area + radar charts
- **Lucide React** — Icon system

---

## 🔧 Setup

### Prerequisites
- Python 3.10+
- Node.js 18+

### 1. Backend
```bash
cd toxicity-app/backend
pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### 2. Frontend
```bash
cd toxicity-app/frontend
npm install
npm run dev -- --force
```

### 3. Open
```
Frontend → http://localhost:5173
API Docs → http://localhost:8000/docs
```

---

## 📡 API Reference

### `POST /analyze`
Analyze a single text message.

```json
// Request
{ "text": "You are such a waste of space", "threshold": 0.3 }

// Response
{
  "risk_score": 88.0,
  "labels": { "Threat": 0.03, "Hate Speech": 0.09, "Insult": 0.88, "Obscenity": 0.03, "Sarcasm": 0.03 },
  "highlights": ["waste"],
  "processing_time_ms": 52.4
}
```

### `POST /analyze-bulk`
Batch analyze up to 500 texts in parallel.

```json
// Request
{ "texts": ["text1", "text2", "..."], "threshold": 0.3 }

// Response
{
  "results": [...],
  "total": 42, "toxic_count": 8, "safe_count": 34,
  "avg_risk_score": 23.5, "processing_time_ms": 210.0
}
```

### `POST /analyze-file`
Upload a `.txt`, `.csv`, or `.json` file.

```
multipart/form-data: file=<binary>, threshold=0.3
```

### `POST /analyze-url`
Fetch and analyze a web page.

```json
// Request
{ "url": "https://en.wikipedia.org/wiki/Hate_speech", "threshold": 0.3 }
```

---

## 📊 Performance

| Metric | Value |
|---|---|
| Single inference | ~50ms |
| Bulk 100 texts | ~200ms (parallel) |
| URL fetch + analysis | ~1–3s |
| Frontend bundle size | < 500KB |
| Memory usage | < 100MB |

---

## 🎯 Use Cases

1. **Social Media Platforms** — Auto-moderate user comments in real-time
2. **Customer Support** — Flag aggressive/threatening support tickets
3. **Gaming** — Chat moderation in multiplayer games
4. **Content Review** — Pre-publication content screening
5. **Research** — Batch analyze datasets for toxicity patterns

---

## 👥 Team

Built for **Hackathon 2026** by Team Shield

---

## 📄 License

MIT License — free to use, modify, and distribute.
