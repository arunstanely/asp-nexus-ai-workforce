<p align="center">
  <img src="screenshots/banner.png" alt="ASP Nexus Banner" width="100%"/>
</p>

<h1 align="center">🤖 ASP Nexus AI Workforce</h1>
<p align="center">
  <b>Intelligent Web Form Automation — OCR · Multi-AI · Playwright</b><br/>
  Electron Desktop App + Chrome Extension · Two versions for two use cases
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%20Desktop-blue?style=flat&logo=windows"/>
  <img src="https://img.shields.io/badge/Extension-Chrome-yellow?style=flat&logo=google-chrome"/>
  <img src="https://img.shields.io/badge/Automation-Playwright-green?style=flat"/>
  <img src="https://img.shields.io/badge/OCR-Tesseract%20%2F%20Mistral-orange?style=flat"/>
  <img src="https://img.shields.io/badge/AI-Gemini%20%7C%20Claude%20%7C%20OpenAI%20%7C%20Ollama-purple?style=flat"/>
  <img src="https://img.shields.io/badge/Status-Built-brightgreen?style=flat"/>
</p>

---

## 🧩 The Problem

Enterprise data entry is one of the most expensive, error-prone, and soul-crushing tasks in any organization. Insurance companies, HR departments, and government portals still require operators to manually transfer data from physical documents into web forms — field by field, every day.

A single data entry operator handles hundreds of forms. Every form is slightly different. Errors mean rejections. Rejections mean rework.

**ASP Nexus automates this entirely — read a document, map the fields, fill the form.**

---

## 🎯 Two Versions — Two Use Cases

### Version 1 — Chrome Extension (Side Panel)
A lightweight browser extension that runs as a **side panel alongside any webpage**. The operator loads a document (PDF/image), the extension extracts text via OCR, maps fields to the visible form, and fills them — without leaving the browser.

- ✅ Zero installation friction for end users
- ✅ Pre-configured field mapping for specific high-volume forms (e.g. insurance portals)
- ✅ Operator reviews mapped fields before submission
- ⚠️ Requires one-time configuration per unique form layout

**First deployed for:** Insurance claim entry portal

### Version 2 — Electron Desktop App (Universal)
A full desktop application that **opens any webpage in a managed Playwright browser**, scans the DOM automatically, maps form fields using AI fuzzy matching, and populates them — with a mandatory review step before execution.

- ✅ Works on any webpage — no pre-configuration needed
- ✅ AI-powered field mapping adapts to unknown form layouts
- ✅ Multi-provider AI (switch between Gemini, Claude, OpenAI, Ollama)
- ✅ Full audit trail in SQLite

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                    Electron Shell                             │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                React + TypeScript UI                   │  │
│  │                                                        │  │
│  │  Document Upload → OCR Preview → Field Mapping Review  │  │
│  │         → Confirm → Execute → Audit Log                │  │
│  └───────────────────────────┬────────────────────────────┘  │
└──────────────────────────────│───────────────────────────────┘
                               │
          ┌────────────────────▼──────────────────────┐
          │              Main Process                  │
          │                                            │
          │  ┌──────────┐  ┌──────────┐  ┌─────────┐  │
          │  │   OCR    │  │  AI Field│  │Playwright│  │
          │  │ Engine   │  │  Mapper  │  │ Runner  │  │
          │  │Tesseract │  │(Multi-AI)│  │(Browser)│  │
          │  │ /Mistral │  │          │  │         │  │
          │  └──────────┘  └──────────┘  └─────────┘  │
          │                      │                     │
          │              ┌───────▼──────┐              │
          │              │   SQLite DB  │              │
          │              │ (Prisma ORM) │              │
          │              └──────────────┘              │
          └─────────────────────────────────────────────┘
```

---

## 🔬 Technical Decisions & Challenges

### 1. Mandatory Review Screen — No Silent Execution
The biggest risk in form automation is wrong data being submitted silently. Every run goes through a **MappingReviewPanel** — a dedicated screen showing exactly which document field maps to which form field, with confidence scores. The user must confirm before Playwright touches anything.

### 2. Multi-Provider AI Architecture
Different customers have different AI access. The field mapping engine supports **Gemini, Claude (Anthropic), OpenAI, and Ollama (local)** — switchable from settings without code changes. This was architected as a provider-agnostic interface from the start.

### 3. React Crash — Double JSON.parse Bug
During Sprint 3, the app crashed silently on field mapping results. Root cause: the AI response was being JSON.parse'd twice — once in the FastAPI layer and once in the React state update. The fix required adding an explicit type guard before parsing and a fallback error boundary component.

### 4. Chrome Extension CSP Bundling
The Chrome Extension version ran into Content Security Policy violations when bundling Webpack-compiled scripts. Solved by restructuring the build to produce CSP-compliant output and moving inline scripts to external files.

### 5. DOM Scanning for Unknown Forms
For the Electron version to work on any page, it needed to reliably identify form fields regardless of how they were coded. The DOM scanner uses a scoring heuristic — `label` text, `placeholder`, `name`, `aria-label`, `id` — weighted and passed to the AI for fuzzy matching against document fields.

---

## 📱 Screenshots

| Document Upload | Field Mapping Review | Playwright Execution | Audit Log |
|---|---|---|---|
| ![Upload](screenshots/upload.png) | ![Review](screenshots/review.png) | ![Execute](screenshots/execute.png) | ![Audit](screenshots/audit.png) |

> 📹 **Demo Video:** [Watch on YouTube](https://youtube.com/your-link-here)

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Desktop Shell | Electron |
| Frontend | React + TypeScript + Vite |
| OCR Engine | Tesseract.js / Mistral Vision API |
| AI Field Mapper | Gemini · Claude · OpenAI · Ollama (switchable) |
| Browser Automation | Playwright |
| Database | SQLite via Prisma ORM |
| Chrome Extension | Manifest V3 · Side Panel API |

---

## 🗺️ Roadmap

- [x] Chrome Extension — insurance portal configuration
- [x] Electron App — universal DOM scanner + multi-AI mapper
- [x] Mandatory review screen before execution
- [x] SQLite audit trail
- [ ] Batch processing — multiple documents in queue
- [ ] Role-based access for team environments
- [ ] SaaS web version

---

## 👤 Author

**Arun Stanely Prakash G** — Founder, SISAI Edutech
[LinkedIn](https://linkedin.com/in/arunstanely) · [GitHub](https://github.com/arunstanely)

---

## 📄 License

Proprietary — All Rights Reserved. Source code not included in this repository.
