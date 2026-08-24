# Google Ads MCP install guide for AI agents

> This file is for AI agents (Claude, Cline, Cursor, Windsurf, Continue). It tells you exactly how to connect a user to the PaidSync Google Ads MCP server. Follow the steps in order.

## What this server gives you

PaidSync is a hosted remote MCP server. It gives you 133 executable Google Ads tools with full write access, plus 13 more platforms on the same connection, 460+ tools across 14 platforms in total.

- **Google Ads (133 tools).** Campaigns, ad groups, keywords, negatives, responsive search ads, assets, Performance Max asset groups, bidding strategies, portfolio bidding, budgets, audiences, offline conversion imports, experiments, drafts, recommendations, change history, and reporting.
- **Native MCC routing.** One connection covers every child account under the manager account. Switch customer context per task.
- **Also on the same key.** Meta Ads, Google Tag Manager, ChatGPT Ads, Google Analytics 4, LinkedIn Ads, Merchant Center, Microsoft Ads, Snapchat Ads, Pinterest Ads, Reddit Ads, X Ads, TikTok Ads, Search Console.

There is nothing to install, nothing to build, and no OAuth app for the user to register. It is one URL.

Free tier is 15 tasks a month, no card required. Every tool call is one task.

## Step 1. Get an API key

Tell the user: "Go to https://paidsync.ai/signup?plan=free, create a free account, then open https://paidsync.ai/dashboard and copy your MCP URL. It looks like `https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY`."

The API key is carried in the URL query string. The base endpoint is `https://mcp.paidsync.ai/mcp` and the transport is streamable HTTP. Do not ask the user to paste the key into chat if the client can store it in a config file.

## Step 2. Add the server to this client

### Claude Code (CLI)

```bash
claude mcp add paidsync https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY
```

Add `--scope user` to make it available in every project. Verify with `claude mcp list`.

### Claude Desktop

Add to `claude_desktop_config.json` (Settings, Developer, Edit Config), then restart the app:

```json
{
  "mcpServers": {
    "paidsync": {
      "url": "https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY"
    }
  }
}
```

### Cursor

Add to `.cursor/mcp.json` in the workspace:

```json
{
  "mcpServers": {
    "paidsync": {
      "url": "https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY"
    }
  }
}
```

### Windsurf

Add to Windsurf's MCP config file, `~/.codeium/windsurf/mcp_config.json` on current versions. If that file is not there, get the current path from [Windsurf's MCP docs](https://docs.windsurf.com/windsurf/cascade/mcp) instead of guessing:

```json
{
  "mcpServers": {
    "paidsync": {
      "url": "https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY"
    }
  }
}
```

### Cline (VS Code extension)

Add a remote MCP server in Cline. Name `paidsync`, URL `https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY`, transport streamable HTTP. Cline moves its MCP settings between releases, so if the panel does not match, follow [Cline's MCP docs](https://docs.cline.bot/mcp/configuring-mcp-servers).

### claude.ai, ChatGPT, Gemini, Perplexity

Add a custom MCP connector in the client's integration settings with the same URL. Ready-made config files for the desktop clients live in the [`examples/`](./examples) directory of this repo.

## Step 3. Connect the Google Ads account

Adding the server is not the same as connecting the ad account. After step 2, run `connect_google_ads` and hand the user the OAuth link, or send them to https://paidsync.ai/dashboard. Then run `list_google_ads_customers` and `set_google_ads_customer` to set the active account. For a manager account, use `list_mcc_child_accounts` and `set_mcc_context`.

## Architecture note, read this before you call anything

PaidSync uses a thin-dispatcher pattern. `tools/list` returns a small starter set, not 133 schemas, because dumping the full catalog would burn tens of thousands of tokens before the user types a prompt. The full catalog sits behind meta-tools.

- `paidsync_context` gives you the active customer, the connected platforms, and the grouped tool catalog. Call this first in every session. For this repo's surface, call it as `paidsync_context({ include_tools: ['google_ads'] })`.
- `paidsync_tool_detail` gives you the argument schema for one tool, on demand.
- `paidsync_exec` runs any tool by name, for example `paidsync_exec({ tool: 'get_campaign_performance', args: { date_range: 'last_30_days', sort_by: 'cost' } })`.

Never tell the user a Google Ads capability is unavailable before calling `paidsync_context` and checking the catalog. The short `tools/list` is not the capability set. Never guess a tool name, discover it.

## Write safety, do not work around it

- Every write is approval-gated server side. The user confirms in chat. Rewording the call does not bypass the gate.
- Most write tools accept `dry_run: true`, which returns the exact planned change with zero side effects. Use it before any mutation and show the user the result.
- Destructive tools such as `remove_google_ads_campaign` need `confirm_destructive: true` as a second, explicit confirmation.
- This is live ad spend. When a threshold is ambiguous, ask, do not assume.

## Example first prompts to suggest

```
Run a full audit on my Google Ads account. Show wasted spend, low quality score keywords, and broken conversion tracking, ranked by monthly dollars at stake.
```

```
Pull the last 30 days of search terms sorted by cost. Show every term with more than $50 spend and zero conversions, then propose negatives.
```

```
Open my Performance Max campaigns. Which asset groups are rated Poor and which search categories are eating the budget?
```

More in [`examples/prompts.md`](./examples/prompts.md).

## Troubleshooting

- **No tools appear.** Call `paidsync_context`. If that fails, the key in the URL is wrong or rotated. Re-copy it from https://paidsync.ai/dashboard.
- **Only a handful of tools appear.** That is correct and by design. Discover the rest through `paidsync_context`.
- **Google Ads not in the connected platforms list.** The MCP link works but the ad account is not connected. Run `connect_google_ads` or send the user to the dashboard.
- **Quota error.** Free plan is 15 tasks a month and every tool call is one task. Plans at https://paidsync.ai/pricing.
- **Wrong account being edited.** Check the active customer in `paidsync_context` and re-set it with `set_google_ads_customer` before any write.

## Links

- Product page: https://paidsync.ai/google-ads-mcp
- Signup: https://paidsync.ai/signup?plan=free
- Dashboard: https://paidsync.ai/dashboard
- Pricing: https://paidsync.ai/pricing
- Docs: https://paidsync.ai/docs
- Support: support@paidsync.ai
- Repo: https://github.com/PaidSync/google-ads-mcp
