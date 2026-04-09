# Registrar's Assistant — Pinata Custom Agent Template

A [Pinata custom agent](https://github.com/PinataCloud/agent-template) that assists a credential registrar. You stay in charge; the assistant drives the [credcli ClawHub skill](https://clawhub.ai/ntbooks/credcli) on your behalf to issue Chainletter credentials on demand from chat (Telegram, etc.) or in batches via mail merge. No custom server, no alternate renderers.

## What it does
- **Single-recipient issuance** — "issue a Web3 101 certificate to alice@example.com" → agent builds a 1-row CSV and runs the credcli pipeline, returns a claim URL.
- **Mail merge** — drop in a CSV, agent validates against the template's field map and issues the whole batch.
- **Template design help** — create or revise Chainletter templates with Claude: generate and edit artwork, size to spec, clean up logos, plan layouts. Design assets are then uploaded into Chainletter through its normal template editor.
- **Hard line at issuance time** — credentials a recipient receives are *only* produced by credcli, never by any other renderer.

## Before deploying
You must provide:
1. **CREDCLI IPFS CID** — copy from [clawhub.ai/ntbooks/credcli](https://clawhub.ai/ntbooks/credcli) and replace `<CREDCLI_CID>` in [manifest.json](manifest.json).
2. **`CREDCLI_TOKEN`** — Chainletter API token (Pinata secret).
3. **`ISSUER_NAME`** and **`ISSUER_LOGO_URL`** — set as Pinata secrets.
4. **At least one Chainletter template** created in your Chainletter workspace (the registrar will discover it via `credcli template list`).
5. *(Optional)* **`DEFAULT_TEMPLATE_ID`** — fallback template when a chat request doesn't name one.
6. *(Optional)* **`TELEGRAM_BOT_TOKEN`** — from @BotFather, if you want Telegram as a channel. Skip for web chat only.

## Layout
```
.
├── manifest.json
├── README.md
├── .gitignore
└── workspace/
    ├── BOOTSTRAP.md   # first-run checklist the agent self-executes
    ├── SOUL.md        # registrar personality
    ├── IDENTITY.md    # name + emoji
    ├── AGENTS.md      # workspace rules: credcli only, never substitute
    ├── TOOLS.md       # credcli command reference
    ├── USER.md        # blank — fill in at first run
    └── HEARTBEAT.md   # empty (no periodic tasks)
```

## Verifying
1. Push to Pinata. Agent boots with no build/start scripts and loads credcli from its CID.
2. From your chat channel: "list available credential templates" → expect a `credcli template list` reply.
3. "Issue a test certificate to <name> at <email> using template <id>" → expect a claim URL back.
4. Open the claim URL — confirm it matches Chainletter's standard claim-page format.
5. Upload a small (3-row) CSV → confirm all three recipients are issued and the job ID is reported.
