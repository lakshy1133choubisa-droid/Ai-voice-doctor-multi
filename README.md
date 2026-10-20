# Ai-voice-doctor-multi
Markdown
# AuraDx Lab — Multimodal Clinical Decision Support (CDS) Platform

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Gradio UI](https://img.shields.io/badge/UI-Gradio-orange.svg)](https://gradio.app/)
[![Gemini 2.5/Flash](https://img.shields.io/badge/LLM-Gemini%20API-blueviolet.svg)](https://ai.google.dev/)
[![Whisper Audio](https://img.shields.io/badge/STT-Whisper%20v3%20via%20Groq-green.svg)](https://groq.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **AuraDx Lab** is an enterprise-grade, multimodal AI Clinical Decision Support (CDS) system designed to ingest medical imagery, cine video loops, structured laboratory data, and voice dictations to synthesize structured clinical assessments and spoken doctor briefings across 5 languages.

---

## 📌 Executive Summary & Recruiter Highlights

- **Multimodal AI Orchestration:** Integrates multi-frame DICOM/CT/MRI/X-Ray imaging, video endoscopy/ultrasound, lab documents (PDF, CSV, Excel), and speech input.
- **Deterministic Clinical Guardrails:** Implements algorithmic KDIGO 2024 renal staging and MRI/CT contrast safety gates (NSF/CIN gating) before model synthesis.
- **Resilient AI Pipeline:** Built-in model failover cascading, self-healing JSON parsers, and FHIR-aligned schema validation via Pydantic.
- **Multilingual Patient Briefing:** Generates native spoken briefings in English, German, Spanish, Arabic, and Hindi using Whisper STT and gTTS with EU regulatory compliance disclaimers.

---

## 🏗️ System Architecture & End-to-End Flow

                 ┌──────────────────────────────────────────┐
                 │           Multimodal User Inputs         │
                 │  (Voice, DICOM/Images, Video, Lab Docs)  │
                 └────────────────────┬─────────────────────┘
                                      │
               ┌──────────────────────┴──────────────────────┐
               │                                             │
               ▼                                             ▼
    [ Groq Whisper STT ]                           [ Ingestion Engine ]
(Speech-to-Text Transcription)                • DICOM Windowing (Adaptive HU)
│                             • Video Slicing (Keyframes)
│                             • Lab Parsing (PDF, CSV, Excel)
│                                             │
└──────────────────────┬──────────────────────┘
│
▼
┌───────────────────────────────────────┐
│    Deterministic Clinical Filters     │
│   • KDIGO 2024 Renal / Dialysis Engine│
│   • MRI/CT NSF & Contrast Safety Gate │
└───────────────────┬───────────────────┘
│
▼
┌───────────────────────────────────────┐
│       Subspecialty Domain Router      │
│  (Oncology, Thoracic, Cutaneous,      │
│   Neuro/Head, Abdomen, MSK, Dialysis) │
└───────────────────┬───────────────────┘
│
▼
┌───────────────────────────────────────┐
│       Gemini Inference Cascade        │
│  (Strict Pydantic JSON Schema Output) │
└───────────────────┬───────────────────┘
│
▼
┌───────────────────────────────────────┐
│    Post-Inference Safety Circuit      │
│  • Self-Healing JSON Repair           │
│  • Invasive Hardware Assertion Check  │
└───────────────────┬───────────────────┘
│
┌──────────────────────┴──────────────────────┐
│                                             │
▼                                             ▼
[ Diagnostic Synthesis ]                      [ Localized TTS Engine ]
Structured Clinical Report                     Audio Briefing (12–15 Sentences)
+ Evidence-based Citations                     + EU Regulatory AI Disclaimer


---

## 🔑 Key Engineering & Clinical Features

### 1. Subspecialty Prompt Routing
Automatically directs clinical context into specialized expert pathways based on anatomical landmarks and modality:
- **Oncology & Neoplasm:** NCCN 2024 guidelines, TNM staging recommendations, biopsy contraindications.
- **Cardiothoracic:** Hemithorax asymmetry detection, pleural effusion, anterior mediastinum evaluation.
- **Dermatology:** ABCDE lesion criteria, inflammatory dermatoses triage.
- **Neuroradiology & Head/Neck:** Boston Criteria v2.0 for Cortical Superficial Siderosis (cSS/CAA), acute stroke differentials.
- **Nephrology & Dialysis:** FMC/KDIGO adequacy audits (spKt/V, UFR, VP pressure monitoring).

### 2. Pre-Inference Renal & Contrast Safety Gating
Prevents unsafe AI recommendations by checking renal function metrics before contrast imaging analysis:
- **Nephrogenic Systemic Fibrosis (NSF) Check:** Flags linear gadolinium MRI contrast if eGFR < 30 mL/min/1.73m² or ESRD on dialysis.
- **Contrast-Induced Nephropathy (CIN) Check:** Enforces non-contrast alternatives or isotonic pre-hydration protocols.

### 3. Fault-Tolerant Resilience
- **Model Fallback Cascade:** Gracefully transitions across tier models (`gemini-3.5-flash` → `gemini-3.5-flash-lite` → `gemini-3.0-pro`) in case of rate limits or service unavailability.
- **JSON Self-Healer:** Automatically repairs truncated JSON closures (mismatched brackets, quotes) during token limits before passing into Pydantic validators.

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Frontend UI** | [Gradio](https://gradio.app/) | Responsive dashboard with custom medical theme CSS |
| **Language & Reasoning** | [Google GenAI SDK](https://github.com/google-gemini/generative-ai-python) | Multimodal inference, structured extraction |
| **Speech-to-Text** | [Groq API](https://groq.com/) (`whisper-large-v3`) | Ultra-fast audio dictation transcription |
| **Text-to-Speech** | [gTTS](https://github.com/pndurette/gTTS) | Native voice briefing delivery across 5 languages |
| **Data Validation** | [Pydantic v2](https://docs.pydantic.dev/) | Strict data contracts aligned with FHIR DiagnosticReport |
| **Medical Imaging** | `pydicom`, `OpenCV`, `Pillow` | DICOM ingestion, Hounsfield Unit (HU) windowing |
| **Document Processing** | `pypdf`, `pypdfium2`, `pandas` | Scanned/structured lab report OCR and table extraction |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10 or higher
- Gemini API key
- Groq API key (for audio transcription)

### Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/](https://github.com/)<your-username>/auradx-lab.git
   cd auradx-lab
