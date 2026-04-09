# Tools

## credcli (provided by skill)
The Chainletter credcli is installed automatically by the credcli skill declared in `manifest.json`. Its postinstall step also downloads Chromium (~130 MB) into `~/.credcli/browsers` — this is expected.

Network access required: `*.chainletter.io`, `*.clstamp.com`.

Read `SKILL.md` from the credcli skill for the authoritative command reference. Common verbs:
- `credcli auth login --token <token>`
- `credcli workspace set --name <name> --logo <url>`
- `credcli workspace get`
- `credcli template list`
- `credcli job create --template <id> --csv <path>`
- `credcli render <jobId>`
- `credcli upload <jobId>`
- `credcli stamp <jobId>`

## Chat channels
Telegram is declared in `manifest.json` (gated on `TELEGRAM_BOT_TOKEN`). Discord and Slack can be added the same way if needed.

## Filesystem
- Use `~/.credcli/jobs/<short-id>/` as the working directory for any CSVs you generate. Don't scatter files elsewhere.
- Use `~/.credcli/design/<short-id>/` for any artwork you produce while helping the registrar design templates.

## Template filename convention
credcli encodes a template's pixel dimensions in its filename: the last token of the basename, before the extension, is `WIDTHxHEIGHT`. Examples:

- `diploma-classic-2480x3508.png` → 2480 × 3508
- `badge-circle-1024x1024.png` → 1024 × 1024
- `certificate-landscape-3300x2550.png` → 3300 × 2550

When designing or revising a template, read the existing filename to learn the required canvas size, and save new assets with the same `name-WIDTHxHEIGHT.ext` pattern so credcli picks them up correctly.
