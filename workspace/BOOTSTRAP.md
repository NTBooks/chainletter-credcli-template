# Bootstrap

Run this checklist on first boot, and any time the workspace looks uninitialized.

## 1. Check for saved configuration

Read `USER.md`. If the fields **CREDCLI_TOKEN**, **Issuer name**, and **Issuer logo URL** are already filled in, skip to step 3.

## 2. Gather configuration interactively

These values are short-lived or user-specific, so they are collected in chat rather than deployed as secrets.

Ask the registrar for each of the following, one at a time:

1. **CREDCLI_TOKEN** — "Please paste your Chainletter API token. You can generate one at chainletter.io. (Tokens are short-lived, so you may need to provide a new one in future sessions.)"
2. **Issuer name** — "What is the display name for your issuing organization?"
3. **Issuer logo URL** — "What is the public URL to the logo shown on issued credentials?"
4. **Default template ID** *(optional)* — "Do you have a default Chainletter template ID you'd like to use? If not, just say skip."

As the registrar provides each value, write it into `USER.md` so it persists across conversations. Do **not** ask for all four at once — collect them one by one so the registrar can ask questions or correct mistakes along the way.

## 3. Authenticate credcli

```
credcli auth login --token "<token from USER.md>"
```

If the token is expired or invalid, tell the registrar and ask for a fresh one.

## 4. Configure the workspace identity

```
credcli workspace set --name "<issuer name>" --logo "<issuer logo URL>"
```

## 5. List available templates and remember them

```
credcli template list
```

Store the IDs and human names in memory so future chat requests can be matched against them.

## 6. Validate default template (if set)

If a default template ID is saved in `USER.md`, confirm it appears in the template list. If it doesn't, warn the registrar.

---

After bootstrap, idle and wait for credential requests. Chat channels (Telegram, Discord, Slack, web) are configured at the Pinata layer, not in this manifest.
