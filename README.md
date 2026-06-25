# PA Template

A reusable template for building a Hermes Agent personal assistant — built on
[Hermes Agent](https://hermes-agent.nousresearch.com).

One **concierge** front door routes to multiple specialist personas. Persistent
semantic memory via Supabase, Google Calendar integration, Telegram gateway —
deployable 24/7 on Railway.

> This repo is a **declarative rebuild kit** — configs, souls, skills, and the
> Railway deploy. **No secrets** are committed; supply them via Railway env vars
> (see `.env.example`). Clone this template, customise the SOULs and profiles
> for your use case, and deploy.

## Personas

| Persona | Domain |
|---------|--------|
| concierge | Calendar, daily briefings, travel, meeting prep, communications |
| manufacturing | Sandpaper manufacturing, import/export, sales, finance, strategy |
| trading | Sandpaper manufacturing, import/export, sales, finance, strategy |
| sales | Sandpaper manufacturing, import/export, sales, finance, strategy |

> Replace the above with your own personas. See `profiles/` directory.

## Layout

| Path | What |
|---|---|
| `config.yaml`, `SOUL.md` | concierge (default profile) config + routing soul |
| `profiles/<name>/` | specialist personas (config delta + soul) |
| `plugins/memory/supabase/` | custom Supabase memory provider |
| `skills/` | custom skills (e.g. entrepreneur-frameworks) |
| `scripts/supabase_migration.sql` | one-time Supabase schema (tables, pgvector, search fns) |
| `scripts/heartbeat.py` | in-container heartbeat check for cron |
| `scripts/cron-jobs.md` | cron job definitions (morning briefing + heartbeat) |
| `railway/railway-init.sh` | boot-time materialisation of git/Google creds from env |
| `tests/` | automated repo structure tests |
| `.github/workflows/` | CI (tests on push) + uptime monitor (Telegram heartbeat every 15 min) |

## Setup

1. **Clone this template** into a new repo.
2. **Customise** `SOUL.md` and `profiles/*/SOUL.md` with your agent's name,
   owner details, domains, and tone.
3. **Create a Telegram bot** via @BotFather → get the token.
4. **Create a Supabase project** → run `scripts/supabase_migration.sql` in the
   SQL editor.
5. **Get an LLM API key** (Ollama Cloud, OpenRouter, Anthropic, OpenAI, etc.).
6. **(Optional) Set up Google Calendar OAuth** — see `.env.example` for details.
7. **Deploy to Railway**:
   ```bash
   railway up --detach
   ```
8. **Set env vars in Railway** (see `.env.example` for the full list).

The Dockerfile.railway clones the Hermes Agent source from GitHub at build time,
overlays your custom configs, and builds — no local Hermes source needed.

## Environment Variables (set in Railway)

See `.env.example` for the full list. Key variables:
- `TELEGRAM_BOT_TOKEN` — from @BotFather
- `TELEGRAM_ALLOWED_USERS` — the owner's Telegram user ID
- `OLLAMA_API_KEY` / `OLLAMA_BASE_URL` — Ollama Cloud for the LLM (or swap for your provider)
- `SUPABASE_MEMORY_URL` / `SUPABASE_MEMORY_KEY` — Supabase project for memory
- `GCP_OAUTH_KEYS_B64` / `GCAL_TOKENS_B64` — Google Calendar OAuth (base64-encoded)

## Customisation Checklist

- [ ] Rename agent in `SOUL.md` and all `profiles/*/SOUL.md`
- [ ] Replace owner name and description in `SOUL.md`
- [ ] Define your domains/personas in `SOUL.md` routing section
- [ ] Create/rename `profiles/` directories to match your personas
- [ ] Update `tests/test_repo_structure.py` to match your profile names
- [ ] Set tone and language preferences in `SOUL.md`
- [ ] Update `config.yaml` model/provider if not using Ollama Cloud
- [ ] Set all env vars in Railway
