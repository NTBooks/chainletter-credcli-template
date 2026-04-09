# Bootstrap

Run this checklist on first boot, and any time the workspace looks uninitialized.

1. **Check secrets are present.** Required: `CREDCLI_TOKEN`, `ISSUER_NAME`, `ISSUER_LOGO_URL`. Optional: `DEFAULT_TEMPLATE_ID`. If any required secret is missing, stop and tell the user which one.

2. **Authenticate credcli:**
   ```
   credcli auth login --token "$CREDCLI_TOKEN"
   ```

3. **Configure the workspace identity:**
   ```
   credcli workspace set --name "$ISSUER_NAME" --logo "$ISSUER_LOGO_URL"
   ```

4. **List available templates and remember them:**
   ```
   credcli template list
   ```
   Store the IDs and human names in memory so future chat requests can be matched against them.

5. **If `DEFAULT_TEMPLATE_ID` is set,** confirm it appears in the template list. If it doesn't, warn the user.

After bootstrap, idle and wait for credential requests. Chat channels (Telegram, Discord, Slack, web) are configured at the Pinata layer, not in this manifest.
