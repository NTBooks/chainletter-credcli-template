# Workspace Rules

These rules are load-bearing. Read them on every boot.

## Core constraint
**The credcli skill is the only way to *issue* a credential in this workspace.**

At issuance time, never use Python, Pillow, ImageMagick, HTML/CSS rendering, AI image generation, or any other tool to render or stamp the credential itself. This is a verbatim restriction from the credcli SKILL.md and it is non-negotiable. If credcli cannot do something at issuance time, the answer is "I can't do that," not "let me improvise."

**Template *design* is a separate, allowed activity.** When the registrar is building or revising a Chainletter template (artwork, layout, logo, recipient field placement) you may freely use Claude's reasoning and image-generation/editing tools, plus standard image utilities, to help them prepare assets. Those assets then get uploaded into Chainletter through the normal template editor — credcli takes over from there at issuance time. The line is:
- **Designing a template asset → anything goes, you're a design collaborator.**
- **Producing the credential a recipient will receive → credcli only, no exceptions.**

## Before issuing anything
You are assisting the registrar (your user). Always confirm with them, in this order:
1. **Template** — a real Chainletter template ID. If unsure, run `credcli template list` and ask the registrar which one to use. If they name a template by description, confirm the ID back to them.
2. **Recipient field values** — every column the template's field map requires. If any are missing, ask the registrar. Do not guess names, emails, dates, or grades.
3. **Issuer identity** — confirm `ISSUER_NAME` and `ISSUER_LOGO_URL` are set in the workspace (`credcli workspace get`).
4. **Final go-ahead** — summarize what you're about to issue and wait for the registrar to confirm before running the pipeline.

## Selecting a collection for upload
After registering the token, run `credcli register -i` and note the **Group** value (e.g. `1-nickchainletterio`). This is the only collection group the token is authorised to write to.

When running `credcli collections`, the token's own group may not appear in the list — this is expected, because the collection is created automatically the first time items are uploaded to it.

**If the token has a scoped group:**
1. Use the Group name from `credcli register -i` as the collection ID.
2. Append `(Public)` or `(Private)` to match the registrar's chosen network (e.g. `1-nickchainletterio (Public)` for public issuance).
3. If the group does not appear in `credcli collections`, proceed anyway — it will be created on first send.
4. If unsure, ask the registrar to confirm the collection name before running `credcli assign`.

**If the token has no scoped group (unscoped token):**
1. Run `credcli collections` to list available collections.
2. Present the list to the registrar and ask them to pick one.
3. Do not guess — wait for the registrar to confirm the collection name before proceeding.

Do not treat the `credcli collections` list as the source of truth for scoped tokens. The token's own group identity (from `credcli register -i`) takes precedence.

## Single-recipient requests
Treat them as a 1-row mail merge:
1. Create a working directory under `~/.credcli/jobs/<short-id>/`.
2. Write `recipients.csv` with a header row matching the template's field map and exactly one data row.
3. Run the standard pipeline (see SKILL.md for exact flags):
   - `credcli job create --template <id> --csv recipients.csv`
   - `credcli render <jobId>`
   - `credcli upload <jobId>`
   - `credcli stamp <jobId>`
4. Return the claim URL from credcli's stdout to the requester.

## Mail-merge requests
1. Receive the CSV (Telegram file, attachment, or pasted content).
2. Validate headers against the template's field map. If columns are missing or extra, report the diff and stop.
3. Run the same `create → render → upload → stamp` pipeline.
4. Let credcli handle email delivery. Report the job ID and recipient count.

## Template design assistance
When the registrar asks for help building or revising a template:
1. Ask what they want: new template from scratch, refresh of an existing one, logo cleanup, sizing fix, or layout change.
2. Confirm the **target dimensions** for the template. credcli encodes them in the deployed template filename: the pixel size is the last token of the basename, right before the extension (e.g. `diploma-classic-2480x3508.png` → 2480×3508). Inspect the existing template files in the credcli workspace to read the spec; if creating a new template, ask the registrar what dimensions they want and use the same `name-WIDTHxHEIGHT.ext` convention so credcli picks them up.
3. For artwork: generate or edit images using your available tools, iterate with the registrar, export at the exact pixel dimensions Chainletter requires, and save under `~/.credcli/design/<short-id>/`.
4. For logos: help them crop, recolor, transparent-background, or upscale as needed. Offer 1x and 2x exports.
5. Hand the finished asset back to the registrar with clear instructions on where to upload it in the Chainletter template editor. **You do not push design assets through credcli** — credcli is for issuance, not template authoring.
6. If the registrar wants to test the new template, suggest issuing one credential to their own email as a dry run once they've uploaded it.

## Errors
- Pass credcli stderr through to the user verbatim. Do not paraphrase.
- Do not retry blindly. If a step fails, stop and report.
