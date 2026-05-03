<p align="center">
  <img src="public/favicon.svg" width="80" alt="Sahaay AI logo" />
</p>

<h1 align="center">Sahaay AI</h1>

<p align="center">
  <b>Voice-first, camera-powered AI companion for the visually impaired</b><br/>
  Built for Bengaluru Hackathon 2026
</p>

<p align="center">
  <img src="https://img.shields.io/badge/React-18.3-61DAFB?logo=react&logoColor=white" alt="React" />
  <img src="https://img.shields.io/badge/Vite-5.3-646CFF?logo=vite&logoColor=white" alt="Vite" />
  <img src="https://img.shields.io/badge/Express-5-000000?logo=express&logoColor=white" alt="Express" />
  <img src="https://img.shields.io/badge/SQLite-3-003B57?logo=sqlite&logoColor=white" alt="SQLite" />
  <img src="https://img.shields.io/badge/TailwindCSS-4-06B6D4?logo=tailwindcss&logoColor=white" alt="TailwindCSS" />
  <img src="https://img.shields.io/badge/License-MIT-green" alt="MIT License" />
</p>

---

## 🧭 Problem Statement

Over **39 million** people worldwide are blind, with India accounting for nearly **one-third** of that population. Daily tasks like reading medicine labels, identifying currency notes, and recognising faces remain significant barriers to independence.

**Sahaay AI** bridges this gap with a smartphone-only solution — no special hardware required. It uses the device camera and microphone to describe surroundings, read printed text aloud, identify Indian banknotes, and recognise familiar faces — all through natural voice interaction.

---

## ✨ Key Features

| Feature | How It Works | Offline? |
|---|---|---|
| 🌐 **Scene Description** | Camera captures frame → Gemini 2.0 Flash Lite generates spoken description | ❌ |
| 📄 **Text Reading (OCR)** | Tesseract.js extracts text (English, Hindi, Kannada) → spoken aloud with smart medicine label summarisation | ✅ |
| 💰 **Currency Detection** | TensorFlow.js + Teachable Machine classifies Indian banknotes from camera feed | ✅ |
| 👤 **Face Recognition** | face-api.js matches live camera against locally registered contacts | ✅ (local) |
| 🎙 **Voice Commands** | Web Speech API for hands-free interaction — tap the orb, speak naturally | ✅ |
| 🌍 **Multilingual** | Supports English, Hindi (हिंदी), and Kannada (ಕನ್ನಡ) for both input and output | ✅ |
| 🔒 **Offline Mode** | OCR and currency detection work without internet; cloud features gracefully degrade | ✅ |
| 🧡 **Caregiver Dashboard** | Remote monitoring, emergency contacts, and query history for caregivers | ❌ |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    React 18 + Vite SPA                  │
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌────────────┐            │
│  │ OrbScreen│  │ Settings │  │ Caregiver  │  ...more    │
│  └────┬─────┘  └──────────┘  └────────────┘            │
│       │                                                 │
│  ┌────▼──────────────────────────────┐                  │
│  │           useAI() hook            │ ← central hub    │
│  │  ┌─────────┐ ┌─────────┐         │                  │
│  │  │ useMic()│ │useVision│         │                  │
│  │  └─────────┘ └────┬────┘         │                  │
│  └───────────────────┼──────────────┘                  │
│                      │                                  │
│  ┌───────────────────▼──────────────────────┐          │
│  │            Vision Pipeline                │          │
│  │  ┌──────────┐ ┌────────┐ ┌────────────┐  │          │
│  │  │Tesseract │ │TF.js + │ │ face-api.js│  │          │
│  │  │  OCR     │ │Teach.M.│ │            │  │          │
│  │  └──────────┘ └────────┘ └────────────┘  │          │
│  └──────────────────────────────────────────┘          │
│                      │                                  │
│           (cloud fallback if offline fails)             │
│    ┌─────────────────▼───────────────────┐             │
│    │  Gemini 2.0 Flash  │  Groq Vision   │             │
│    └─────────────────────────────────────┘             │
└────────────────────┬────────────────────────────────────┘
                     │ /api/*
          ┌──────────▼──────────┐
          │   Express 5 Server  │
          │   + SQLite (better  │
          │     -sqlite3)       │
          │                     │
          │  • User profiles    │
          │  • Query logs       │
          │  • Emergency        │
          │    contacts         │
          │  • Face registry    │
          └─────────────────────┘
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 18
- **Chrome** or **Edge** (required for Web Speech API)
- API keys for cloud vision (optional — offline features work without them)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/ApoorvaPoojary8/Bengaluru--Hakathon.git
cd Bengaluru--Hakathon

# 2. Install dependencies
npm install

# 3. Configure environment variables
cp .env.example .env
# Edit .env and add your API keys:
#   VITE_GEMINI_API_KEY=your_gemini_key
#   VITE_GROQ_API_KEY=your_groq_key (optional fallback)
#   VITE_OPENAI_API_KEY=your_openai_key (optional)

# 4. Start the backend
node server/index.js

# 5. Start the frontend (in a new terminal)
npm run dev
```

The app will be available at `http://localhost:5173`.

### Environment Variables

| Variable | Required | Description |
|---|---|---|
| `VITE_GEMINI_API_KEY` | Yes | Google Gemini 2.0 Flash Lite API key |
| `VITE_GROQ_API_KEY` | No | Groq API key (vision fallback) |
| `VITE_OPENAI_API_KEY` | No | OpenAI API key (additional fallback) |
| `VITE_API_BASE_URL` | No | Production backend URL (Vite proxies `/api/*` → `localhost:3001` in dev) |

---

## 🧪 Testing

```bash
# Run all tests
npm test

# Run with UI
npm run test:ui

# Watch mode
npm run test:watch
```

**Test coverage includes:**
- ✅ Prompt structure and validation
- ✅ TTS speech synthesis integration
- ✅ Face recognition model loading and matching
- ✅ Currency classification pipeline
- ✅ Backend API endpoints (27 tests)

---

## 📂 Project Structure

```
├── public/
│   ├── currency-model/        # Teachable Machine exports
│   ├── face-models/           # face-api.js model weights
│   └── favicon.svg
├── server/
│   ├── index.js               # Express 5 API server
│   ├── db.js                  # SQLite schema & bootstrap
│   └── test/                  # API integration tests
├── src/
│   ├── hooks/
│   │   ├── useAI.js           # Central AI orchestration hook
│   │   ├── useMic.js          # Web Speech STT hook
│   │   └── useVision.js       # Camera + vision pipeline
│   ├── utils/
│   │   ├── openai.js          # Vision API client (Gemini/Groq)
│   │   ├── tts.js             # Text-to-speech with Chrome keepalive
│   │   ├── currency.js        # ONNX/TF.js banknote classifier
│   │   ├── faceMatch.js       # face-api.js face recognition
│   │   └── prompts.js         # Mode-specific AI prompts + demo scripts
│   ├── screens/
│   │   ├── OrbScreen.jsx      # Main orb interface
│   │   ├── DemoScreen.jsx     # Live demo walkthrough
│   │   ├── SettingsScreen.jsx # User preferences
│   │   ├── CaregiverScreen.jsx# Caregiver dashboard
│   │   ├── QuickActionsScreen.jsx
│   │   └── OnboardingScreen.jsx
│   ├── test/                  # Vitest test suites
│   ├── App.jsx                # Router root
│   ├── main.jsx               # Entry point
│   └── index.css              # Global design tokens
├── docs/                      # Manual test checklists
├── .env.example               # Environment template
├── vite.config.js             # Vite + Vitest config
└── package.json
```

---

## 🎯 How It Works

1. **Tap the Orb** — The glowing 3D orb activates voice input
2. **Speak naturally** — "What do you see?", "Read this for me", "How much is this note?"
3. **AI processes** — Sahaay captures a camera frame and routes to the appropriate pipeline:
   - **Scene mode** → Cloud vision API describes the surroundings
   - **OCR mode** → Local Tesseract.js reads text (with medicine label detection)
   - **Currency mode** → TF.js classifies Indian banknotes offline
   - **Face mode** → face-api.js matches against registered contacts
4. **Hear the response** — Result is spoken aloud via Web Speech TTS

---

## 🛡 Offline-First Design

Sahaay is designed for India's connectivity reality:

- **OCR** runs entirely in-browser via Tesseract.js (English, Hindi, Kannada)
- **Currency detection** uses a TensorFlow.js model — no internet needed
- **Face matching** works locally with face-api.js descriptor comparison
- When offline, cloud-dependent features (Scene, Face description) show clear user-friendly error messages
- The offline toggle in Settings lets users force local-only processing

---

## 🔧 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18, Vite 5, Tailwind CSS v4 |
| **AI/ML (Local)** | Tesseract.js 5, TensorFlow.js 4, face-api.js, ONNX Runtime Web |
| **AI/ML (Cloud)** | Google Gemini 2.0 Flash Lite, Groq Vision (fallback) |
| **Speech** | Web Speech API (STT + TTS) with Chrome keepalive fix |
| **Backend** | Express 5, better-sqlite3 |
| **Testing** | Vitest, Testing Library, Supertest |

---

## 👥 Team

Built with ❤️ and passion by **Team Sahaay** at Bengaluru Hackathon 2026.

| Member | Role |
|---|---|
| Apoorva Poojary | Full-Stack Development & Integration |

---

## 📜 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

---

<p align="center">
  <i>"Technology should be an equalizer, not a divider."</i><br/>
  <b>Sahaay AI — Your eyes, your voice, your independence!</b>
</p>
