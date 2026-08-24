# Claude Code setup

PaidSync is a hosted remote MCP server. There is nothing to install and nothing to build. You add one URL.

## 1. Get your key

Sign up at [paidsync.ai/signup](https://paidsync.ai/signup?plan=free). Free plan is 15 tasks a month, no card required. Copy your MCP URL from [paidsync.ai/dashboard](https://paidsync.ai/dashboard). It looks like `https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY`.

## 2. Add the server

```bash
claude mcp add paidsync https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY
```

To make it available in every project instead of just this one, add the user scope flag.

```bash
claude mcp add --scope user paidsync https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY
```

## 3. Verify

```bash
claude mcp list
```

You should see `paidsync` with a connected status. For the full record including the URL and transport:

```bash
claude mcp get paidsync
```

Then verify from inside Claude Code. Start a session, type `/mcp`, and confirm `paidsync` is listed. Now run the real check, which is a live call:

```
Call paidsync_context with include_tools ['google_ads'] and tell me the active customer, the connected platforms, and how many Google Ads tools came back.
```

A healthy connection returns your active Google Ads customer, the list of platforms you have connected, and the grouped Google Ads catalog. That is 1 task against your quota.

## 4. Connect Google Ads

If `paidsync_context` returns no Google Ads customer, the MCP link is fine but the ad account is not connected yet. Ask for it in chat.

```
Connect my Google Ads account.
```

Then follow the OAuth link, and after that run `list_google_ads_customers` and `set_google_ads_customer` to pick the account you want active.

## Troubleshooting

- **`paidsync` missing from `claude mcp list`.** The add command ran in a different directory and used local scope. Re-run it with `--scope user`.
- **Connected but no tools.** Expected. PaidSync deliberately exposes meta-tools, not 133 raw schemas. Call `paidsync_context` first, then `paidsync_exec`.
- **401 or key errors.** The key in the URL is wrong or rotated. Copy it again from the dashboard, then `claude mcp remove paidsync` and add it back.
- **Quota errors.** Free plan is 15 tasks a month. Every tool call is one task. Plans at [paidsync.ai/pricing](https://paidsync.ai/pricing).
