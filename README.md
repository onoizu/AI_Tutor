# Adaptive AI Tutor

A **premium learning companion interface** powered by the **Coze platform**. This application is not a standalone LLM stack—it is a **Next.js front end** that orchestrates pedagogy, session state, and rich UI around a **production-grade Coze Agent**, exposed through the official **Coze Chat API**. The result is an adaptive tutor experience: structured explanations, quizzes, repair flows, session summaries, and a study studio—all driven by your configured agent intelligence.

**Coze Agent (store link):**  
[Open the connected agent on Coze](https://www.coze.com/store/agent/7621548317553967157?bot_id=true)

The UI layer translates conversational turns into typed **`TutorResponse`** payloads, normalizes the agent’s JSON contract, and renders a **three-column workspace**—quick learning actions, live chat with mode-aware cards, and a right-hand “brain” plus notebook—while `/api/tutor` securely proxies requests to Coze with your credentials.

---

## Requirements

- **Node.js** 18+ (20 LTS recommended)
- **npm** (or pnpm / yarn)

---

## Install

```bash
cd adaptive-ai-tutor-agent
npm install
```

---

## Local development

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

### Project layout

| Path | Purpose |
|------|---------|
| `src/app/` | App Router pages and layout (`page.tsx` entry) |
| `src/components/layout/` | `MainLayout`, `LeftSidebar`, `CenterPanel`, `RightSidebar` |
| `src/components/cards/` | Teach / quiz / repair / summary / mind map cards |
| `src/app/api/tutor/` | Server route: forwards messages to **Coze** |
| `src/lib/api.ts` | Client `sendMessage` → `/api/tutor` |
| `src/lib/cozeClient.ts` | Coze HTTP client and config validation |
| `src/types/tutor.ts` | `TutorResponse` and related types |

---

## Build & production preview

```bash
npm run build
npm start
```

Use `npm start` to verify the production build locally before deployment.

---

## Deploy on Vercel

1. Import this repo (or use `adaptive-ai-tutor-agent` as the project root) into [Vercel](https://vercel.com).
2. Set **Framework Preset** to **Next.js**; default build/output settings are fine.
3. Add the same Coze variables as in `.env.local` under **Environment Variables**, then redeploy.

> **Security:** Never commit real PATs. Keep tokens only in Vercel env vars or local `.env.local`.

---

## Coze environment variables (`.env.local`)

Create `.env.local` at the project root (do not commit):

```bash
# Required: personal access token from the Coze console (often pat_...)
COZE_API_TOKEN=pat_xxxxxxxx

# Required: numeric Bot / Agent ID
COZE_BOT_ID=your_numeric_id
# Alias supported in code:
# COZE_AGENT_ID=your_numeric_id

# Optional: API region (China default https://api.coze.cn; global example)
# COZE_API_BASE_URL=https://api.coze.com
```

**Common mistake:** Using an OAuth page URL or any `https://...` string as `COZE_API_TOKEN`. The token must be a **plain PAT string**; the codebase validates obvious misconfigurations.

---

## Lint

```bash
npm run lint
```

---

## Extending

- Centralize session IDs, streaming, and fallbacks in `src/lib/api.ts` (`sendMessage`).
- Mount the full tutor experience from `src/app/page.tsx` for demos or coursework deliverables.
