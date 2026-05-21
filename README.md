# 🧠 AI Projects — Shanu Choudhary

> Production AI systems I've designed and built independently — from autonomous marketing agents to voice-first GenAI for Indian farmers.

**I'm an AVP (SDE III) at JPMorgan Chase by day.** These are my personal learning projects where I explore agentic AI, knowledge architectures, and multilingual LLM systems — shipping real products to real users.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Shanu_Choudhary-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/shanu-connect/)
[![Medium](https://img.shields.io/badge/Medium-@chshanu97-black?style=flat&logo=medium)](https://medium.com/@chshanu97)
[![Portfolio](https://img.shields.io/badge/Portfolio-shanu--web.github.io-green?style=flat)](https://shanu-web.github.io/shanu-portfolio/)

---

## 📌 Projects Overview

| Project | What It Does | Stack | Live |
|---------|-------------|-------|------|
| [Amelytic](#-amelytic--autonomous-ai-marketing-platform) | Autonomous AI marketing — plan, create, publish, analyze | LangGraph, OpenAI GPT-4o, FastAPI | [amelytic.com](https://amelytic.com) |
| [AmelyticForge](#-amelyticforge--ai-agent-builder) | Train AI agents on any data, deploy to WhatsApp/Web | LangChain, Meta Graph API, FastAPI | [app.amelytic.com](https://app.amelytic.com) |
| [PashuGuru](#-pashuguru--voice-first-genai-for-indian-farmers) | Voice-first livestock advisor in 6 Indian languages | LangChain, Google Voice API, FastAPI | [pashushala.com](https://pashushala.com) |

---

## 🟣 Amelytic — Autonomous AI Marketing Platform

**The Problem:** Small businesses spend 15-20 hours/week on marketing — planning content, writing posts, scheduling across platforms, analyzing results. Most can't afford a marketing team.

**The Solution:** An agentic AI workspace that autonomously plans campaigns, generates channel-native content across 13+ platforms, routes through approval workflows, publishes on schedule, and surfaces performance analytics.

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    AMELYTIC PLATFORM                         │
│                                                             │
│  ┌──────────┐   ┌──────────────┐   ┌────────────────────┐  │
│  │ Campaign  │──▶│   Content    │──▶│    Approval        │  │
│  │ Planner   │   │  Generator   │   │    Workflow         │  │
│  │  Agent    │   │   Agent      │   │    Engine           │  │
│  └──────────┘   └──────────────┘   └────────────────────┘  │
│       │                │                     │              │
│       │         ┌──────▼──────┐        ┌─────▼─────┐       │
│       │         │  Channel    │        │ Scheduler  │       │
│       │         │  Adapters   │        │ + Auto-    │       │
│       │         │  (13+)      │        │ Publisher  │       │
│       │         └─────────────┘        └───────────┘       │
│       │                                      │              │
│       │              ┌───────────────────────▼──────┐       │
│       └──────────────│     Analytics Dashboard      │       │
│                      └──────────────────────────────┘       │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              LangGraph Orchestration Layer            │   │
│  │         (Multi-Agent State Machine + Routing)         │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                    OpenAI GPT-4o                      │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### How It Works

1. **Campaign Planner Agent** — Takes brand context + business goals → generates a content calendar with topics, channels, and timing
2. **Content Generator Agent** — Produces channel-native content (LinkedIn carousel ≠ Instagram reel ≠ email newsletter) from the same brief
3. **Approval Workflow** — Human-in-the-loop review before anything goes live
4. **Auto-Publisher** — Publishes approved content on schedule across connected platforms
5. **Analytics** — Tracks performance, feeds insights back into future planning

### Key Design Decisions

| Decision | Why |
|----------|-----|
| **LangGraph over single-prompt chains** | Campaign planning requires multi-step reasoning with state management. LangGraph's state machine handles conditional routing (e.g., "if brand has no Instagram, skip reel generation") |
| **Channel adapters as separate modules** | Each platform has different content specs (character limits, image ratios, hashtag rules). Adapters normalize output per channel without polluting the generation logic |
| **Human-in-the-loop approval** | Fully autonomous publishing is risky for brand reputation. The approval step is intentional friction — it catches hallucinated claims before they go live |

### Tech Stack

`Python` `FastAPI` `LangGraph` `LangChain` `OpenAI GPT-4o` `AWS` `Terraform` `PostgreSQL` `Redis`

---

## 🔧 AmelyticForge — AI Agent Builder

**The Problem:** Businesses want AI chatbots trained on *their* data — not generic ChatGPT. But building a custom AI agent requires ML expertise most businesses don't have.

**The Solution:** A no-code AI agent builder. Point it at a website, PDF, or database → it trains an agent → deploy as a web widget or WhatsApp bot in 25+ languages with native script rendering.

### Architecture — Brand Brain 🧠

This is the core innovation. I wrote about why I built this in detail: **[Why Baseline RAG Fails in Production](https://medium.com/@chshanu97/why-baseline-rag-fails-in-production-and-what-i-built-instead-f8235c02ac57)**

Standard RAG: `chunk → embed → retrieve → generate`. Works in demos, fails in production.

Brand Brain adds two critical layers between raw data and the LLM:

```
┌──────────────────────────────────────────────────────────────┐
│                     BRAND BRAIN ARCHITECTURE                  │
│                                                              │
│   LAYER 1 — INGESTION                                        │
│   ┌────────────────────────────────────────────────────┐     │
│   │  Website Crawler  │  PDF Parser  │  Database Sync  │     │
│   │                                                    │     │
│   │  Extracts: content + metadata + WHAT'S NOT COVERED │     │
│   └──────────────────────┬─────────────────────────────┘     │
│                          │                                    │
│   LAYER 2 — LLM COMPILER  ◀── This doesn't exist in RAG     │
│   ┌──────────────────────▼─────────────────────────────┐     │
│   │  Raw content → Structured Brand Knowledge Graph     │     │
│   │                                                     │     │
│   │  • What the brand KNOWS (grounded assertions)       │     │
│   │  • What the brand OFFERS (products/services)        │     │
│   │  • What the brand DOESN'T offer (explicit gaps)     │     │
│   │  • Cross-references between topics                  │     │
│   │                                                     │     │
│   │  Every assertion grounded in source material.       │     │
│   │  Gaps explicitly flagged — NOT silently ignored.    │     │
│   └──────────────────────┬─────────────────────────────┘     │
│                          │                                    │
│   LAYER 3 — RUNTIME                                          │
│   ┌──────────────────────▼─────────────────────────────┐     │
│   │  User Query                                         │     │
│   │    → Query compiled knowledge (NOT raw chunks)      │     │
│   │    → Gap check: "Do I know this?" (yes/no)          │     │
│   │    → If yes: generate grounded answer                │     │
│   │    → If no: decline gracefully. No hallucination.    │     │
│   └──────────────────────┬─────────────────────────────┘     │
│                          │                                    │
│   LAYER 4 — FEEDBACK LOOP                                    │
│   ┌──────────────────────▼─────────────────────────────┐     │
│   │  • Unanswered queries → flagged as knowledge gaps   │     │
│   │  • Source drift detection → auto re-compilation     │     │
│   │  • Usage patterns → inform business owner           │     │
│   │    "14 users asked about X. You should add this."   │     │
│   └────────────────────────────────────────────────────┘     │
│                                                              │
│   Result: Knowledge COMPOUNDS over time.                     │
│   Baseline RAG is static. Brand Brain is self-improving.     │
└──────────────────────────────────────────────────────────────┘
```

### Why Brand Brain > Baseline RAG

| Problem | Baseline RAG | Brand Brain |
|---------|-------------|-------------|
| **Hallucination** | Retrieves "close enough" chunks → LLM extrapolates → wrong answer | Compiler flags gaps at build time → runtime declines if it doesn't know |
| **Stale data** | Embeddings from old content serve outdated info | Feedback loop detects source drift → triggers re-compilation |
| **Inconsistency** | Stateless — same question, different answers | Compiled knowledge graph → same source, same answer |
| **No self-improvement** | Static forever | Unanswered queries feed back as knowledge gaps |

### WhatsApp Integration

Deployed AI agents to WhatsApp Business Cloud API via Meta Graph API. Built the integration from scratch after Meta Developer Console kept crashing — used direct API calls with System User token generation.

```
User (WhatsApp) → Meta Webhook → FastAPI Server → Brand Brain → Response → Meta API → User
```

Supports 25+ languages in native script (Devanagari, Arabic, Tamil, etc.)

### Tech Stack

`Python` `FastAPI` `LangChain` `LangGraph` `OpenAI API` `Pinecone` `AWS` `Terraform` `WhatsApp Business Cloud API` `Meta Graph API` `JavaScript`

---

## 🐄 PashuGuru — Voice-First GenAI for Indian Farmers

**The Problem:** 300M+ Indian livestock farmers have limited access to veterinary advice. Most speak regional languages. Most are not comfortable typing. Existing solutions are text-based and English-first.

**The Solution:** A voice-first AI veterinary advisor. Farmers speak in their language → AI understands → generates guidance → speaks back. Vet escalation under 15 minutes for complex cases.

### Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    PASHUGURU                              │
│                                                         │
│   🎙️ VOICE INPUT (Farmer speaks)                        │
│   ┌───────────────────────────────────────────────┐     │
│   │  Google Speech-to-Text (STT)                  │     │
│   │  Supports: Hindi, Marathi, Tamil,             │     │
│   │  Telugu, Kannada, Bengali                     │     │
│   └────────────────────┬──────────────────────────┘     │
│                        │                                 │
│   🧠 AI PROCESSING                                      │
│   ┌────────────────────▼──────────────────────────┐     │
│   │  LangChain + OpenAI                           │     │
│   │                                               │     │
│   │  • Veterinary knowledge base                  │     │
│   │  • Symptom analysis                           │     │
│   │  • Treatment recommendations                  │     │
│   │  • Emergency detection → vet escalation       │     │
│   └────────────────────┬──────────────────────────┘     │
│                        │                                 │
│   🔊 VOICE OUTPUT                                       │
│   ┌────────────────────▼──────────────────────────┐     │
│   │  Google Text-to-Speech (TTS)                  │     │
│   │  Response in farmer's language                │     │
│   └───────────────────────────────────────────────┘     │
│                                                         │
│   🚨 ESCALATION                                         │
│   ┌───────────────────────────────────────────────┐     │
│   │  Complex case detected                        │     │
│   │  → Vet notified within 15 minutes             │     │
│   │  → Farmer gets callback                       │     │
│   └───────────────────────────────────────────────┘     │
│                                                         │
│   Live at: pashushala.com                               │
└─────────────────────────────────────────────────────────┘
```

### Why This Matters

- **300M+ livestock farmers** in India — most without reliable vet access
- **Voice-first** — because typing in Devanagari on a smartphone is hard for many farmers
- **6 languages** — not translated English, but native understanding
- **15-minute vet escalation** — AI handles routine queries, humans handle emergencies
- **Zero revenue** — this is a social impact project, not a business

### Design Decisions

| Decision | Why |
|----------|-----|
| **Voice-first, not text-first** | Target users are more comfortable speaking than typing. Voice removes the literacy barrier. |
| **Google STT/TTS over Whisper** | Better support for Indian regional languages. Whisper is great for English, weaker for Marathi/Kannada. |
| **Vet escalation built-in** | AI should know its limits. Life-threatening symptoms → don't generate advice, escalate to human. This is the "knowing what you don't know" principle from Brand Brain. |

### Tech Stack

`Python` `FastAPI` `LangChain` `OpenAI API` `Google Cloud Speech-to-Text` `Google Cloud Text-to-Speech`

---

## 🔗 Published Writing

- **[Why Baseline RAG Fails in Production — And What I Built Instead](https://medium.com/@chshanu97/why-baseline-rag-fails-in-production-and-what-i-built-instead-f8235c02ac57)** — Deep dive into Brand Brain architecture and the three failures of baseline RAG

---

## 🛠️ Common Tech Across Projects

```
Languages:       Python (primary), JavaScript
Frameworks:      FastAPI, LangChain, LangGraph
LLM:             OpenAI GPT-4o (function calling, embeddings, structured outputs)
Vector Store:    Pinecone, pgvector
Cloud:           AWS (Lambda, ECS, S3), Terraform, Docker
APIs:            WhatsApp Business Cloud API, Meta Graph API, Google Cloud APIs
Orchestration:   LangGraph multi-agent state machines
Architecture:    RAG, Brand Brain (custom), agentic workflows
```

---

## 📬 Contact

- **Email:** chshanu97@gmail.com
- **LinkedIn:** [linkedin.com/in/shanu-connect](https://www.linkedin.com/in/shanu-connect/)
- **Medium:** [medium.com/@chshanu97](https://medium.com/@chshanu97)
- **Portfolio:** [shanu-web.github.io/shanu-portfolio](https://shanu-web.github.io/shanu-portfolio/)

---

*These are all my personal learning projects. All code, architecture decisions, and deployments are my own.*
