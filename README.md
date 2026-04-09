# Registrar's Assistant — Pinata Agent Template

Issue blockchain-signed credentials from chat. Certificates, diplomas, badges, licenses — each one rendered from your template, stamped on-chain, and delivered as a unique claim link the recipient can verify independently. No code to write, no infrastructure to manage.

This [Pinata custom agent](https://github.com/PinataCloud/agent-template) pairs an AI assistant with the [credcli ClawHub skill](https://clawhub.ai/ntbooks/credcli) to give you a conversational registrar's desk backed by [Chainletter](https://chainletter.io) credentials.

## Why

PDF certificates can be forged. Email confirmations get lost. A Chainletter credential is a tamper-proof, blockchain-stamped record that anyone can verify by following a link — no special software, no account required. This agent lets you issue those credentials as easily as sending a chat message.

## What it does

- **Issue from chat** — "Issue a Web3 101 certificate to alice@example.com" and the agent handles the rest: builds the CSV, renders the credential from your template, uploads it, stamps it on-chain, and hands back a claim URL.
- **Batch issuance** — drop in a CSV of recipients and the agent validates it against your template's fields and issues the entire batch in one pipeline run.
- **Template design** — create or revise credential artwork with Claude's help: generate backgrounds, size images to spec, clean up logos, plan field layouts. Then upload the finished assets to Chainletter's template editor.
- **Verifiable by anyone** — every issued credential gets a permanent claim link. Recipients and third parties can confirm authenticity without contacting you.

## Before deploying

You need:
1. A **Chainletter account** at [chainletter.io](https://chainletter.io) with at least one credential template.
2. The **credcli skill CID** — copy from [clawhub.ai/ntbooks/credcli](https://clawhub.ai/ntbooks/credcli) and paste into [manifest.json](manifest.json).
3. A **`CREDCLI_TOKEN`** — your Chainletter API token (set as a Pinata secret).
4. **`ISSUER_NAME`** and **`ISSUER_LOGO_URL`** — your organization's display name and logo (Pinata secrets).
5. *(Optional)* **`DEFAULT_TEMPLATE_ID`** — a fallback template when a chat request doesn't name one.

Chat channels (Telegram, Discord, Slack, web) are configured at the Pinata layer after deploy.

## Quick start

1. Fork this repo and push to Pinata.
2. Connect a chat channel (Telegram, Discord, Slack, or web).
3. Send: "List available credential templates" — you should see your Chainletter templates.
4. Send: "Issue a test certificate to [name] at [email] using template [id]" — you should get a claim URL back.
5. Open the claim URL and verify it matches Chainletter's standard claim page.
6. Try a batch: upload a small CSV and confirm all recipients are issued.

## Layout

```
.
├── manifest.json
├── README.md
├── .gitignore
└── workspace/
    ├── BOOTSTRAP.md   # first-run checklist the agent self-executes
    ├── SOUL.md        # agent personality and boundaries
    ├── IDENTITY.md    # name + emoji
    ├── AGENTS.md      # workspace rules
    ├── TOOLS.md       # credcli command reference
    ├── USER.md        # filled in at first run
    └── HEARTBEAT.md   # empty (no periodic tasks)
```
