---
description: Run Claritty's discovery method — problem → two ideal outcomes → focused questions → a written brief — for an idea or an existing app
argument-hint: <the problem you want the app to solve>
allowed-tools: Bash, Read, Write, Edit, Glob, Grep
---

You are running **Claritty's discovery method** — the same problem→outcomes→questions
strategy the platform's own builder uses — to turn a fuzzy idea into a sharp brief
*before* any code. Use this standalone (just thinking through an idea) or inside an
already-scaffolded Claritty app to (re)write its brief. Be warm, concise, decisive.

The user's problem (may be empty — ask if so): **$ARGUMENTS**

## 1. Restate the problem
Reflect it back in one line so you're aligned before proposing anything.

## 2. Propose TWO ideal outcomes (the user picks one)
Show where this could land — don't ask "what features?".
- **Parity first:** if the Claritty CLI is signed in, get the platform's real ones —
  run `claritty discover outcomes "<problem>"` (prints one line of JSON:
  `{"outcomes":[{"title","description"}]}`). On `{"unauthenticated":true}` or any
  error, generate the two yourself.
- Make them **DISTINCT:** one **proactive** ("it handles it for me" — schedule/auto),
  one **on-demand** ("it helps me when I act"). ≤6-word title + 1–2 sentences.
  **Self-contained:** the app's own data + built-in Claude + triggers; no "connect
  your account" unless a catalog integration covers it.
- Present both via **AskUserQuestion** (the user may pick one or describe their own).

## 3. Ask 3–5 focused follow-ups (WAIT for answers)
Concrete options, plain language (never *trigger/workflow/agent*), each with a
"Let me specify…" choice.
- **Parity first:** `claritty discover questions "<problem>"` (prints
  `{"questions":[{"prompt","options":[…]}]}`); else generate them yourself.
- Cover at least: the **widget glance** (the ONE thing seen + one tap-action),
  **automation** (cadence + autonomy: autonomous / suggestive / observational), and
  **data** (its own data, or a catalog integration — confirm the obvious service,
  never ask for keys).

## 4. Write the brief
Synthesize problem + **chosen outcome** + the answers into the plan: the
agent/workflow/trigger, the widget per size (170×170 / 360×170 / 360×360), the data
model, and the design identity. If you're in a scaffolded app, save it to
`docs/plans/0001-brief.md` (`mkdir -p docs/plans` first) and set
`app-config.json` → `clarity_marketplace.core_action.definition_of_done` to the
one-sentence success line. Otherwise print the brief and suggest
`npx create-claritty-app@latest <name>` (or `/claritty:new`) to scaffold from it.

Full method + rules: the app's `.claude/prompts/brainstorm.md` (the Discovery Playbook).
