# Agency OS

> The workspace for creative teams — a native desktop application combining project management, brand intelligence, team communication, and multi-model AI.

**Status:** In development — Phase 1  
**Author:** [Sandro Ieva](https://sandroieva.com) · mail@sandroieva.com  
**Live docs:** [agency-os-docs.vercel.app](https://agency-os-docs.vercel.app)  
**Prototype:** [agency-os-prototype.vercel.app](https://agency-os-prototype.vercel.app)

---

## What is Agency OS?

Agency OS is a native macOS/Windows desktop application built for creative agencies, solo practitioners, and micro-studios. It replaces the fragmented workflow of Slack + Notion + Figma + Google Drive + Asana with a single unified experience.

It's not a PM tool with AI bolted on — it's a **brand intelligence platform** with operations and real-time communication attached. The AI knows your brand voice, personas, and guidelines. Every output is on-brand by default.

Think **Obsidian meets aWork** — knowledge management with depth, task delegation with structure, and multi-model AI woven throughout.

---

## Core modules

| Module | Purpose | Key features |
|--------|---------|-------------|
| **Tasks** | Daily execution | Kanban boards, time tracking, task delegation |
| **Chat** | Communication | Project channels, DMs, voice/video calls (LiveKit), @ai mentions |
| **Plan** | Strategic oversight | Calendar, governance, project-level kanban |
| **Files & Brand** | Brand intelligence | Asset vault, brand core, personas, competitor intel |
| **Create** | Creative workbench | Mini apps: brainstorm, campaigns, visuals, templates |
| **Task Force** | AI agents | Autonomous agents with defined skills and brand context |
| **Projects** | Container | Links tasks, chat, files, docs per project |

---

## Tech stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Desktop shell | Tauri 2 (Rust) | 3–8 MB binary, 30–50 MB RAM, sub-second launch |
| Frontend | React 19 + Vite | Fast iteration, component reuse |
| Backend | Supabase (PostgreSQL + Auth + Realtime + Storage) | Instant API, RLS, real-time, EU hosting |
| AI | OpenRouter API | One API key, 300+ models, per-user model selection |
| Voice/video | LiveKit | Open-source WebRTC, free 5000 min/month |
| Collaboration | Tiptap + Yjs | CRDT real-time Markdown editing |
| Animations | Framer Motion | Spring physics, AnimatePresence |
| 3D | Three.js + GLSL shaders | AI sphere with FBM noise gradients |
| Sound | Web Audio API | Synthesized tones, no external files |
| Font | Geist | Sans-serif, clean, modern |

---

## AI integration

Every AI interaction is enriched with brand context before reaching any model:

```
User input → Brand context injector → OpenRouter → Claude / GPT / Gemini / Llama
```

Each team member selects their preferred model. The brand knowledge (tone, personas, guidelines, positioning) is stored in Supabase and injected into the system prompt automatically.

### Where AI appears

- **Voice assistant** — speak naturally, get AI response with animated sphere
- **Chat @ai** — ask questions in any channel
- **Doc editor** — "Draft with AI" button generates content in brand voice
- **Task cards** — "AI expand" turns title into detailed description
- **Brand vault** — generate personas from a brief
- **Task Force** — assign tasks to autonomous AI agents

---

## Architecture

### Database schema (MVP)

```
teams            → name, logo_url, owner_id
profiles         → display_name, avatar_url, email, preferred_model
projects         → name, description, status, deadline, team_id
tasks            → title, status, assignee_id, due_date, priority, project_id
messages         → channel, sender_id, content, attachments, team_id
documents        → title, yjs_state, project_id, team_id
brand_assets     → name, file_url, type, tags, version, team_id
brand_knowledge  → section, content, team_id
files            → name, path, size_bytes, mime_type, project_id
notifications    → type, title, body, read, recipient_id
```

All tables include `id` (UUID), `created_at`, `updated_at`, `user_id`. RLS policies enforced from Phase 3.

### Platform strategy

| Phase | Platform | Technology |
|-------|----------|-----------|
| 1 | macOS | Tauri 2 (WebKit) |
| 4 | Windows | Tauri 2 (WebView2) |
| 5 | iOS | Tauri 2 Mobile (Safari WebView) |
| 5 | Android | Tauri 2 Mobile (Chrome WebView) |

---

## Notifications

Three layers:

| Layer | Technology | Phase |
|-------|-----------|-------|
| In-app | Bell icon, notification center, badge count | Phase 1 |
| Native desktop | Tauri notification plugin, dock badge, sound | Phase 1 |
| Mobile push | Firebase Cloud Messaging, deep links | Phase 5 |

---

## MVP roadmap

### Phase 1 — Foundation (weeks 1–3)
*Single user · macOS only*

- Tauri 2 + React + Vite scaffold
- Supabase project (EU Frankfurt) with Google OAuth
- Dashboard with circular scroll menu and start view
- Tasks module: kanban board, time tracking
- Voice AI: mic → OpenRouter → speech synthesis → sphere
- In-app notifications + native macOS notifications
- Sound design (Web Audio API)

### Phase 2 — Knowledge layer (weeks 4–6)
*Single user*

- Plan module: calendar, governance, project kanban
- Files + Brand: asset vault, brand core, personas, competitor
- Create module: mini apps (brainstorm, campaigns, visuals, templates)
- Markdown documents with Tiptap editor
- Brand knowledge base feeding all AI interactions
- Supabase Storage for files

### Phase 3 — Multi-user + communication (weeks 7–10)

- Team management: invite links, roles (owner/member)
- Chat: project channels, DMs, file sharing (Supabase Realtime)
- Voice + video calls (LiveKit)
- Task delegation with notifications
- Collaborative docs (Yjs CRDT sync)
- Row-Level Security on all tables
- Task Force: AI agents with skills

### Phase 4 — Intelligence + scale (weeks 11–14)

- AI agents: autonomous competitor monitoring, content drafting
- Integrations: Google Drive, Figma API
- Analytics dashboard
- Windows build (Tauri 2)
- Push notifications (FCM)
- iOS + Android companion app

---

## Design language

| Property | Value |
|----------|-------|
| Theme | Dark (#0B0B0F background, #111117 surfaces) |
| Font | Geist (sans-serif) |
| Accent | Purple-pink gradient (sphere), white (active states) |
| Corners | 16–28px on cards, 50% on icons |
| Animations | Framer Motion springs, easing `[0.22, 0.68, 0.35, 1.0]` |
| Sound | Sine wave arpeggios, ascending for positive actions |
| AI sphere | Three.js GLSL — FBM noise, rose-purple-blue palette |
| Menu | Circular segmented ring, scroll navigation, orbital cards |

---

## Project structure

```
agency-os-prototype/
├── src/
│   ├── App.jsx          # Main app (menu, voice AI, dashboard)
│   └── main.jsx         # React entry point
├── api/
│   └── chat.js          # Vercel serverless proxy for AI API
├── index.html           # Entry point
├── package.json         # Dependencies
├── vite.config.js       # Vite config
└── vercel.json          # Deploy config
```

---

## LLM context

If you're an AI assistant helping build this project, here's the essential context:

```
Project:          Agency OS (UI branded as "i7 OS")
Author:           Sandro Ieva — sandroieva.com
Type:             Native desktop app for creative agencies
Stack:            Tauri 2 + React 19 + Supabase + OpenRouter
Design:           Dark theme, Geist font, Framer Motion, Three.js sphere
State mgmt:       Zustand + React Query
Auth:             Google OAuth via Supabase Auth
Files:            Supabase Storage (S3-compatible)
Chat:             Supabase Realtime (MVP), upgrade to Stream Chat later
Calls:            LiveKit (WebRTC, free tier)
Docs:             Tiptap + Yjs (CRDT collaboration)
AI routing:       OpenRouter (one API, all models, per-user selection)
Brand context:    Injected into system prompt from brand_knowledge table
Notifications:    Supabase table + Tauri native + FCM (later)
Build approach:   Design in Figma → vibe code with Claude Code
Current phase:    Phase 1 — single user foundation
```

### Architecture principles

1. **Single user first** — every feature works for one user before adding teams
2. **Brand context injection** — every AI call includes tone, personas, guidelines
3. **Model agnostic** — OpenRouter routes to user's preferred model
4. **Native first** — Tauri APIs for notifications, file system, system tray
5. **Offline resilient** — React Query cache + optimistic updates
6. **Responsive from day 1** — CSS breakpoints for future mobile companion
7. **Schema ready for multi-tenant** — `user_id` / `team_id` on every table from start

---

## Running locally

```bash
# Clone
git clone https://github.com/SandroIeva/agency-os-prototype.git
cd agency-os-prototype

# Install
npm install

# Dev server
npm run dev

# Build
npm run build
```

For the AI voice feature on Vercel, set the environment variable:
```
ANTHROPIC_API_KEY=sk-ant-...
```

---

## License

Private — All rights reserved.

---

*Built by [Sandro Ieva](https://sandroieva.com) with a lot of Claude.*
