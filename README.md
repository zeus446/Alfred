# Alfred

**Your AI butler — quiet, capable, and always one step ahead.**

Alfred is an all-in-one personal assistant that handles the small decisions of daily life so you don't have to keep making them. He curates where you eat, plans where you travel, builds what you eat at home, and keeps your kitchen stocked — learning your taste well enough that most of the time, all you need to do is hit *order*.

Named after the one butler who always seemed to know what you needed before you asked.

---

## What Alfred does

- 🍽️ **Restaurant & Coffee Curator** — Learns your taste and suggests (and can order from) restaurants and coffee spots that actually fit your mood, budget, and cravings.
- ✈️ **Travel Planner** — Builds full trip plans around your preferences, pace, and budget — flights, stays, and a day-by-day itinerary.
- 🥗 **Diet Builder** — Designs and adapts a personal diet plan based on your goals, restrictions, and what you actually like eating.
- 🛒 **Grocery Manager** — Tracks what you need, restocks proactively, and pulls ingredients straight from your diet and meal plans.
- 🎙️ **Voice-first interaction** — Talk to Alfred like you would a real assistant, no typing required.
- 🛍️ **One-tap ordering** — Alfred pre-fills your cart across connected apps based on context and history. You just confirm.

## How he works

Alfred is built as an agentic orchestrator on top of the **Model Context Protocol (MCP)**. Each domain — restaurants, travel, diet, groceries — is its own MCP tool-server. A central agent, built with **PydanticAI**, reasons over your request, pulls context from your preference profile, calls the right tool(s), and hands back a single coherent suggestion or action — not four separate bots bolted together.

The frontend talks to a **FastAPI** backend, which exposes the orchestration agent and handles auth, sessions, and routing to the MCP tool layer underneath. PydanticAI pairs naturally with FastAPI here — both are Pydantic-based, so request/response models and tool schemas stay typed end to end.

```
┌────────────┐      ┌───────────────────────────────────────┐
│  Next.js   │◄────►│              FastAPI                   │
│  Frontend  │      │   (auth, sessions, agent endpoint)     │
└────────────┘      └───────────────┬─────────────────────────┘
                                     │
                     ┌───────────────▼────────────────┐
                     │            Alfred Core          │
                     │   (orchestration agent + memory)│
                     └───┬───────────┬───────────┬─────┘
                         │            │           │
                   ┌─────▼──┐   ┌─────▼───┐ ┌─────▼────┐  ┌──────────┐
                   │Restaur.│   │ Travel  │ │  Diet    │  │ Grocery  │
                   │  MCP   │   │  MCP    │ │  MCP     │  │  MCP     │
                   └────────┘   └─────────┘ └──────────┘  └──────────┘
```

## Tech stack

| Layer | Tech |
|---|---|
| Frontend | Next.js / React |
| Backend API | FastAPI |
| Database | Supabase |
| Agent orchestration | Python, PydanticAI |
| Tool layer | MCP servers (one per domain) |
| Automation | n8n |
| LLM inference | Groq, OpenRouter |
| Voice | Wispr Flow / STT-TTS pipeline (TBD) |

## Project structure

```
/apps
  /web              → Next.js frontend
/services
  /api              → FastAPI backend (auth, sessions, agent endpoint)
  /agents           → orchestration agent (PydanticAI)
  /mcp-servers
    /restaurants
    /travel
    /diet
    /groceries
/infra
  /supabase          → schema, migrations
  /n8n                → automation workflows
```

## Getting started

```bash
git clone https://github.com/zeus446/Alfred.git
cd alfred
cp .env.example .env   # fill in your keys
```

More setup instructions coming as each module is scaffolded.

## Status

🚧 Early build — architecture and first MCP server (Restaurant Curator) in progress.

## Roadmap

- [ ] Restaurant/coffee MCP server + curation logic
- [ ] Preference memory layer
- [ ] Diet builder module
- [ ] Grocery manager + auto-restock
- [ ] Travel planner
- [ ] Voice pipeline integration
- [ ] One-tap cart pre-fill + order confirm flow
