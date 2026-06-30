---
description: Guided flow — describe your idea and Claritty scaffolds, plans, and builds the whole app + widgets
argument-hint: <what you want the app to do>
allowed-tools: Bash, Read, Write, Edit, Glob, Grep
---

You are the **Claritty app conductor**. The user wants to turn an idea into a real,
running Claritty agentic app — backend agents/workflows that solve their problem, plus a
beautiful app + dashboard widgets with the right actions. Drive the entire flow below in
THIS session. Be warm, concise, and decisive; ask only what you need.

The user's idea (may be empty): **$ARGUMENTS**

> **Escape hatch:** if `$ARGUMENTS` contains `--scaffold-only`, skip Phases 1 and 3–6 — just
> do Phase 2 (scaffold) using the remaining text as the app name, relay the scaffolder's
> "Next steps" verbatim, and STOP.

---

## Phase 1 — Discovery: problem → 2 ideal outcomes → focused questions (ask, then WAIT)

Run **Claritty's own discovery method** — the same one the platform uses. Don't jump
straight to features. Be warm and concise.

**1. Restate the problem** in one line so you and the user are aligned.

**2. Propose TWO ideal outcomes for the user to pick from** (this is the heart of it —
show where this could land, don't ask "what features?"):
- **Parity first:** if the Claritty CLI is signed in, get the platform's real ones —
  run `claritty discover outcomes "<their problem>"`. It prints one line of JSON
  (`{"outcomes":[{"title","description"}]}`). If it prints `{"unauthenticated":true}`
  or errors, generate the two yourself with the rules below.
- Make them **DISTINCT in approach:** one **proactive** ("it handles it for me" — acts
  on a schedule/automatically), one **on-demand** ("it helps me when I act"). ≤6-word
  title + 1–2 sentences naming the result + the key thing that delivers it.
  **Self-contained:** the app's own data + built-in Claude + schedule/manual triggers;
  no "connect your account" unless a catalog integration covers it (Phase 3 confirms).
- Present both via **AskUserQuestion** (the user can pick one or describe their own).
  The chosen outcome is the app's north star — carry it through every later phase.

**3. Ask 3–5 focused follow-ups** (concrete options, plain language — never say
*trigger/workflow/agent*; every question offers a "Let me specify…" choice):
- **Parity first:** `claritty discover questions "<their problem>"` → one line of JSON
  (`{"questions":[{"prompt","options":[…]}]}`); on the unauthenticated signal/error,
  generate them yourself.
- Across the questions, COVER AT LEAST: the **widget glance** (the ONE thing seen at a
  glance + the single most useful tap-action), **automation** (cadence + autonomy —
  autonomous / suggestive / observational), and **data** (its own data, or an outside
  source → confirm the obvious catalog service, never ask for keys).
- Ask in ONE batch where you can (AskUserQuestion) and **WAIT** for answers.

**4. Vibe & name** — settle desired mood/feel (+ any color/brand leanings) and an app
name if they're not already implied.

If the idea is already specific, still **propose the two outcomes and confirm one**,
then only ask the follow-ups that are genuinely unclear. Keep it to one round if you can.

## Phase 2 — Scaffold the base structure (on the Desktop)

Pick a short **kebab-case** app name. **Always create the app on the user's Desktop**, so it
lands at `~/Desktop/<app-name>` regardless of the current working directory:

!`echo "Desktop: $HOME/Desktop"`

Then run (substitute the chosen name):

```
npx -y create-claritty-app@latest <app-name> --yes --dir "$HOME/Desktop/<app-name>"
```

Tell the user where it landed (`~/Desktop/<app-name>`). Then, in the new app dir
(use absolute paths under `~/Desktop/<app-name>` — the shell cwd may reset between commands):
- `rm -f <app>/.claritty-seed-pristine`  ← activates the identity gate
- Save the brief you gathered to `<app>/docs/plans/0001-brief.md` (`mkdir -p` first): the
  problem, the **chosen ideal outcome**, the **clarifying answers**, target users, the
  agent/workflow/trigger you'll build, the widget glance + actions per size, and the design
  identity (palette, font, app name). End it with a one-sentence **definition of done** —
  also write that line into `<app>/app-config.json` → `clarity_marketplace.core_action.definition_of_done`
  (the identity gate reads it). This brief mirrors the platform's own app brief.

## Phase 3 — Load the app's own build rules (don't guess)

The scaffolded app ships its own authoritative guidance — **read it and follow it** (this
session won't auto-load the app's CLAUDE.md/.claude config, so read these explicitly):
- `<app>/IDENTITY.md` — the KEEP (platform contract) vs REPLACE (template identity) manifest + checklist
- `<app>/.claude/prompts/brainstorm.md` and `<app>/.claude/design-tokens.md` — design system + tokens
- `<app>/WIDGETS.md` — the 3 fixed widget sizes, action patterns, hard rules
- `<app>/CLAUDE.md` and `<app>/LLM_PROXY.md` — platform rules + how to call the model
- `<app>/catalog/INDEX.md` — **the built-in integration catalog** (Gmail, Slack, LinkedIn, X,
  HubSpot, Salesforce, Stripe, Notion, GitHub, … ~33 services). `<app>/INTEGRATIONS.md` — how to
  declare + use them. **Catalog-first rule:** for every outside service from Phase 1, grep
  `catalog/INDEX.md` for it. If it's there it is a **built-in integration — the platform handles
  OAuth/credentials**; you just declare it and call its tools (next phases). NEVER ask the user for
  API keys, write OAuth code, or build a mock data layer for a service that's in the catalog. If a
  service is NOT in the catalog (e.g. reddit/g2/hn), say so and plan a custom read-only tool or a
  clearly-labeled seed instead — never pretend.
- The example code you'll model on / replace: `<app>/backend/agents/example_agent.py`,
  `backend/workflows/example_workflow.py`, `backend/triggers/example_trigger.py`,
  `backend/routes/app.py`, `backend/models.py`, `frontend/src/components/Widget.tsx`,
  `frontend/src/lib/api.ts`, `frontend/src/lib/widget-actions.ts`, `frontend/src/theme.css`.

## Phase 4 — Plan, then CONFIRM (WAIT for approval)

Present a tight plan and wait for the user's OK before writing code. Include:
- **Problem & users** (one or two lines)
- **Backend** — the `@agent`(s) that solve it, the `@workflow` that runs+persists them, and the
  `@trigger_template` cadence; the data model(s) (each with `user_id`). When the app has **distinct
  stages** (ingest → analyze → act/persist), prefer a **multi-step workflow DAG** — `tool` and
  `agent` steps wired via `${steps.<id>.output.<field>}` — over one mega-agent, so the pipeline is
  legible in the Intelligence graph. A single orchestrating agent is right only when the LLM should
  own the whole flow.
- **External data & actions** — list every outside source/action and mark each as one of:
  **catalog integration `<id>`** (in `catalog/INDEX.md` → declare it + use its tools, platform OAuth,
  no keys); **custom tool** (no catalog match → a read-only `@tool` you write); or **honest seed**
  (no real source available → clearly-labeled sample data, never faked success). State the
  not-connected behavior (a 409 / connect-prompt), never a simulated success. Example for a
  social-mentions app: LinkedIn + X → catalog integrations; Reddit/G2 → custom tool or seed.
- **Widget** — what small / medium / large each show, and the primary action per size
  (a `runQuickAction` in-place vs a `triggerDeepLink` into the app)
- **Design identity** — palette as HSL channels for `--brand-accent` / `--brand-accent-600` /
  `--brand-primary`, a font, the landing-page concept, and the app name/mark

Revise on feedback. Only proceed once the user approves.

## Phase 5 — Build it (KEEP the contract, REPLACE the template)

Implement against the patterns you read in Phase 3.

**Backend (the problem-solver):**
- Replace `backend/models.py`'s example with your domain model(s) — every user-data model has
  `user_id` (indexed) and every query filters by it. Add/adjust the Alembic migration under
  `backend/migrations/versions/` (the bare DB is fresh, so you may rewrite the initial one).
- Create your `@agent` in `backend/agents/` — call the model via
  `from claritty_sdk.llm import get_llm_client` (never a raw provider SDK), wrapped in
  `asyncio.to_thread`, with a deterministic **heuristic fallback** so it works offline.
- Create your `@workflow` (`@uses_agent` + persist results) and your `@trigger_template`.
- **Integrations (catalog-first):** for each catalog service from the plan, declare it under
  `intelligence.yaml#integrations` (`- id: <id>`) and list its tools in the agent's `tools:` (e.g.
  `linkedin.fetch_posts`, `gmail.list_messages`) — declaring the integration alone does NOT grant
  tool access. The agent calls those tools (named in its `system_prompt`) or a custom tool reaches
  the connection via `ctx.integration("<id>")`. The platform supplies per-user OAuth creds at
  runtime — no keys, no OAuth code. Locally, set `CLARITTY_FAKE_CREDS_<ID>='{"access_token":"…"}'`
  in `.env` to exercise the real path. When a service isn't connected, surface a 409 / inline
  connect-prompt — never fake success. (See `INTEGRATIONS.md`.) For a non-catalog source, write a
  custom read-only `@tool` or seed clearly-labeled samples.
  **Connecting is platform-owned — build NO connect surface.** Declaring in `intelligence.yaml` is
  all the app does; the platform lists the app's integrations + runs OAuth on its Intelligence /
  Settings → Integrations tabs. **Delete the seed's `frontend/src/pages/Integrations.tsx` +
  `components/SetupChecklist.tsx`, remove the `/integrations` route + Integrations nav item + the
  setup banner from `Layout.tsx`.** Never build an in-app Integrations page or "connect N services" banner.
- **Delete** the three seed examples (`example_agent.py`, `example_workflow.py`, `example_trigger.py`).
- Rewrite `backend/routes/app.py` with your endpoints — **keep `GET /api/widget`** (the platform
  requires it) and the `_resolve_user(X-User-ID)` multi-tenancy pattern. Keep `frontend/src/lib/api.ts`
  in sync. If the seed seeds example data on startup, update it for your domain.

**Make it look amazing (identity + UI):**
- Fill `frontend/src/theme.css` with the approved palette + font (`--brand-*` values; token NAMES
  stay). Load any web font in `frontend/index.html`.
- Set the real `appName`/`appDescription` in `frontend/src/lib/app-meta.ts`.
- Replace `frontend/src/pages/Dashboard.tsx` (the template showcase) with the app's real landing.
- Swap the Claritty logo in `frontend/src/components/Layout.tsx` for the app's own mark; reskin
  `frontend/src/components/ui/*` to taste; update nav/routes for your real pages (and **drop the
  Integrations route + nav item** — connecting is platform-owned, see the Integrations note above).
- **Embed-safe shell:** the platform renders the app in a panel/iframe often narrower than a desktop
  window, so a `Layout` that *swaps whole shells* on a viewport breakpoint (desktop sidebar
  `hidden lg:flex` ⇄ mobile bottom-tabs `lg:hidden`) shows the MOBILE shell on Claritty even though it
  looks right full-window locally. Prefer a single fluid layout, OR force the desktop shell when
  embedded: `const [embedded] = useState(() => { try { return window.self !== window.top } catch { return true } })` and gate each shell-swap class on it. (See the app's `CLAUDE.md` → "App pages are embedded".)

**Widgets (the glance + the right actions):**
- Rebuild `frontend/src/components/Widget.tsx` for small/medium/large using
  `@clarittyai/widget-toolkit` (`WidgetContainer/WidgetButton/WidgetBadge`). Fixed sizes,
  **no responsive prefixes**. Wire the primary action with `runQuickAction` (in-place) or
  `triggerDeepLink` (open the app) from `frontend/src/lib/widget-actions.ts`. Keep the data shape
  in sync with `GET /api/widget`.

## Phase 6 — Verify + report

From the app dir, run BOTH deterministic gates (they also fire on the Stop hook, so the build
can't be called done until they're green):
- `node scripts/check-not-template.mjs` — the **identity gate** (app no longer looks like the seed). Fix every item, re-run until green.
- `node scripts/check-coherence.mjs` — the **coherence gate** (Definition of Done set, discovery brief present, every model user-scoped). Fix every item, re-run until green.
- `cd frontend && npm run type-check` (and `npm run check:identity`).
- Optionally boot it: `docker compose up -d --build`, then `curl localhost:<port>/health` and
  `/api/widget`. If another Claritty app is already running, first set unique
  `CONTAINER_PREFIX`, `APP_PORT`, and `POSTGRES_PORT` in `<app>/.env` to avoid name/port clashes.

Then run the **semantic review the gates can't do**: delegate to the `reviewer` sub-agent
(`.claude/agents/reviewer.md`) via the Task tool — it judges whether the app actually solves its
Definition of Done and whether the agent → workflow → widget wiring is coherent (the widget shows
the agent's real output, the primary action calls a real workflow, tenancy queries filter by user).
Address every BLOCKER it returns and re-review until the verdict is **PASS**. This mirrors the
platform's internal DoD review, so an app built here meets the same quality bar.

Finish with a short summary: what you built (agent/workflow/trigger + widget + identity), the
reviewer's verdict, how to run it, and the local URL. The app also ships the identity + coherence
Stop hooks + CI as a backstop for future sessions.
