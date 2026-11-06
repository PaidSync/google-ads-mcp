# Google Ads MCP

A Model Context Protocol (MCP) server that lets AI assistants (Claude, ChatGPT, Gemini, Cursor, Perplexity, or any MCP client) run Google Ads end to end. 158 typed tools with full write access, native MCC manager-account routing, approval-gated writes, and 13 more ad and measurement platforms in the same session.

**Hosted at [paidsync.ai](https://paidsync.ai). Free plan: 100 tasks a month, no card required.**

[![MCP](https://img.shields.io/badge/MCP-server-black?logo=anthropic&logoColor=white)](https://modelcontextprotocol.io)
[![Hosted](https://img.shields.io/badge/hosted-paidsync.ai-0077B6)](https://paidsync.ai/google-ads-mcp)
[![Free plan](https://img.shields.io/badge/free%20plan-100%20tasks%2Fmo-7CB518)](https://paidsync.ai/signup?plan=free)
[![Google Partner](https://img.shields.io/badge/Google-Premier%20Partner-4285F4?logo=google)](https://www.google.com/partners/agency?id=7427374971)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

## What this is

PaidSync's Google Ads MCP surface, exposed to any AI assistant that speaks the Model Context Protocol. The full product page lives at [paidsync.ai/google-ads-mcp](https://paidsync.ai/google-ads-mcp). This repository is the developer entry point: install commands, tool catalog, and links to the docs.

Three properties that define this Google Ads MCP:

1. **158 Google Ads tools with full write access.** Not a reporting-only reader. Campaigns, ad groups, keywords, negatives, Performance Max asset groups, bidding strategies, portfolio bidding, audiences, offline conversion imports, experiments, drafts, recommendations, and change history. Every write is a real Google Ads API call.
2. **Native MCC (My Client Center) manager-account routing.** One PaidSync connection covers every child account under the manager. The assistant switches customer context per task, so one key serves the whole manager tree without a second connection per client.
3. **Approval-gated writes with dry-run previews.** Every mutation is held for your confirmation in chat. Most write tools accept `dry_run` to return the exact planned change before touching the account. Destructive operations require an explicit second confirmation.

## Getting started (hosted, recommended)

The fastest path is to connect the hosted server. No install, no OAuth apps, no engineering setup.

### Claude Desktop

Add this to your Claude Desktop MCP configuration:

```json
{
  "mcpServers": {
    "paidsync": {
      "url": "https://mcp.paidsync.ai/mcp"
    }
  }
}
```

No API key is needed: sign in when your client asks. Start free at [paidsync.ai/signup](https://paidsync.ai/signup?plan=free), 100 tasks a month, no card required.

### claude.ai (web) and Claude Code

Go to [claude.ai/settings/integrations](https://claude.ai/settings/integrations) and add a custom MCP integration with URL `https://mcp.paidsync.ai/mcp` and sign in. Same URL works in Claude Code via `claude mcp add paidsync ...`.

### ChatGPT

Add PaidSync as a custom MCP connector in ChatGPT's connector settings, using the same URL as above. The exact menu path moves between ChatGPT releases, so follow [OpenAI's own MCP documentation](https://platform.openai.com/docs/mcp) if the option is not where you expect it.

### Gemini, Cursor, Windsurf, Perplexity, any MCP client

Same URL, same sign-in. Setup guides:
- [Connect Google Ads to Claude](https://paidsync.ai/blog/connect-google-ads-to-claude)
- [Connect Google Ads to ChatGPT](https://paidsync.ai/blog/connect-google-ads-to-chatgpt)
- [Connect Google Ads to Gemini](https://paidsync.ai/blog/connect-google-ads-to-gemini)
- [Connect Google Ads to Perplexity](https://paidsync.ai/blog/connect-google-ads-to-perplexity)
- [Connect Google Ads to Copilot](https://paidsync.ai/blog/connect-google-ads-to-copilot)

## Examples

Copy-paste configs and prompts live in [`examples/`](./examples).

- [`examples/claude_desktop_config.json`](./examples/claude_desktop_config.json), Claude Desktop MCP config
- [`examples/claude_code.md`](./examples/claude_code.md), the exact `claude mcp add` command and how to verify it
- [`examples/cursor_mcp.json`](./examples/cursor_mcp.json), Cursor `.cursor/mcp.json`
- [`examples/windsurf_mcp_config.json`](./examples/windsurf_mcp_config.json), Windsurf `mcp_config.json`
- [`examples/prompts.md`](./examples/prompts.md), 15 paste-ready Google Ads prompts mapped to the tool names below

Machine-readable install steps for an AI agent are in [`llms-install.md`](./llms-install.md). How to report a tool bug or request a tool is in [`CONTRIBUTING.md`](./CONTRIBUTING.md).

## First call

Once connected, PaidSync exposes two meta-tools that keep client context small even with a 630+ tool catalog:

```
paidsync_context({ include_tools: ['google_ads'] })
```

Returns the active customer, the connected platforms, and the full grouped Google Ads catalog with tool names and signatures.

```
paidsync_exec({ tool: 'get_campaign_performance', args: { date_range: 'last_30_days', sort_by: 'cost' } })
```

Runs any tool by name. The assistant handles the two-call pattern automatically; you just ask natural-language questions.

## Tools by category

| Category | Named tools |
|---|---|
| Account and MCC routing | `connect_google_ads`, `list_google_ads_customers`, `set_google_ads_customer`, `list_mcc_child_accounts`, `set_mcc_context` |
| Campaign lifecycle | `list_google_ads_campaigns`, `create_google_ads_campaign`, `update_google_ads_campaign`, `pause_google_ads_campaign`, `enable_google_ads_campaign`, `remove_google_ads_campaign` |
| Ad groups | `list_google_ads_ad_groups`, `create_google_ads_ad_group`, `update_google_ads_ad_group`, `pause_google_ads_ad_group`, `enable_google_ads_ad_group` |
| Keywords and negatives | `list_keywords`, `add_keywords`, `update_keyword`, `remove_keyword`, `add_negative_keywords`, `add_campaign_negative_keywords`, `list_negative_keyword_lists` |
| Search terms | `get_search_terms_report`, `get_ngram_analysis`, `find_wasted_search_terms` |
| Ads and assets | `list_google_ads_ads`, `create_responsive_search_ad`, `update_responsive_search_ad`, `pause_google_ads_ad`, `list_assets`, `create_sitelink_asset`, `create_callout_asset`, `create_structured_snippet_asset`, `link_asset_to_campaign` |
| Performance Max | `list_pmax_campaigns`, `create_pmax_campaign`, `update_pmax_asset_group`, `get_pmax_asset_group_insights`, `get_pmax_search_categories` |
| Bidding and budgets | `list_bidding_strategies`, `create_portfolio_bidding_strategy`, `update_campaign_bidding`, `list_budgets`, `update_campaign_budget`, `get_budget_pacing` |
| Audiences | `list_user_lists`, `create_customer_match_list`, `upload_customer_match_members`, `create_similar_audience`, `list_in_market_segments`, `attach_audience_to_ad_group` |
| Conversions and imports | `list_conversion_actions`, `create_conversion_action`, `import_offline_conversions`, `import_offline_call_conversions`, `get_conversion_diagnostics` |
| Experiments and drafts | `list_experiments`, `create_experiment`, `get_experiment_results`, `promote_experiment` |
| Recommendations and change history | `list_recommendations`, `apply_recommendation`, `dismiss_recommendation`, `get_change_history` |
| Reporting and pacing | `get_campaign_performance`, `get_ad_group_performance`, `get_keyword_performance`, `get_performance_by_device`, `get_performance_by_network`, `get_performance_by_geo`, `get_performance_by_time`, `get_quality_score_report`, `get_auction_insights` |
| Audits | `run_account_audit`, `find_wasted_spend`, `get_pmax_insights`, `get_conversion_tracking_audit` |

Full live catalog with argument signatures is returned by `paidsync_context({ include_tools: ['google_ads'] })`.

## The other 13 platforms in the same session

One PaidSync key covers Google Ads plus 13 more platforms, so the assistant can compare channels and act on them without a second server.

| Platform | Tools |
|---|---:|
| Microsoft Ads | 156 |
| Meta Ads (Facebook + Instagram) | 92 |
| Google Tag Manager | 39 |
| LinkedIn Ads | 34 |
| ChatGPT Ads (OpenAI) | 28 |
| Google Analytics 4 | 26 |
| TikTok Ads | 24 |
| Merchant Center | 16 |
| Snapchat Ads | 15 |
| Pinterest Ads | 14 |
| Reddit Ads | 14 |
| X Ads | 14 |
| Search Console | 6 |

630+ tools across 14 platforms on one endpoint. The counts above are a snapshot, last synced 2026-11-06. The live source is `paidsync_context({ include_tools: '*' })`, which is what to trust if the two disagree.

## Pricing

| Plan | Price | Tasks | Notes |
|---|---|---|---|
| Free | $0/mo | 100 | No card required, all 14 platforms |
| Pro | From $99/mo | 1,000 to 6,000 | Solo operators and small teams |
| Team | From $249/mo | 10,000 shared, up to 20 seats | Shared across the team, MCC-friendly |
| Done For You | Custom | Managed | PaidSync runs the accounts |

Every plan includes all 14 platforms; MCC and Business Manager support start on Pro. No per-account or per-platform charges. Every tool call counts as one task.

## Safety model

- Approval-gated. Every write is a proposal shown in chat before it applies.
- Dry-run previews on most mutations. `dry_run: true` returns the planned change with zero side effects.
- Destructive-write second confirmation. `remove_google_ads_campaign` and similar destructive tools require `confirm_destructive: true`.
- Revocable sessions. Disconnect any account from the PaidSync connect panel at any time.
- Server-side gates, not client-side. The AI cannot bypass the gate by rewording its call.

## Well-known and discovery endpoints

PaidSync publishes the current agent-discovery surface at [paidsync.ai](https://paidsync.ai):

- `/.well-known/mcp/server-card.json` (MCP server card)
- `/.well-known/oauth-authorization-server` (RFC 8414)
- `/.well-known/oauth-protected-resource` (RFC 9728)
- `/.well-known/agent-card.json`
- `/.well-known/agent-auth.json`
- `/llms.txt`, `/llms-full.txt`, `/AGENTS.md`
- Content-Signal directive in `/robots.txt`

## What you get, at a glance

| | PaidSync |
|---|---|
| Google Ads tools | 158 |
| Write access | Full, approval-gated |
| MCC manager-account routing | Yes, native |
| Approval gates on writes | Yes, server side |
| Hosted vs self-hosted | Hosted |
| Platform scope in same session | 14 platforms |
| Google Partner | Google Premier Partner |
| Price | Free $0/mo, Pro from $99/mo, Team from $249/mo |

Head-to-head comparisons against other Google Ads MCP options live at [paidsync.ai/vs/google-ads-mcp](https://paidsync.ai/vs/google-ads-mcp), where they carry a check date. Other people's tool counts, prices, and feature gaps change week to week, so they are maintained there rather than hardcoded in this repo.

## License

MIT. See [LICENSE](./LICENSE).

## Links

- Product page: [paidsync.ai/google-ads-mcp](https://paidsync.ai/google-ads-mcp)
- MCP server overview: [paidsync.ai/mcp-server](https://paidsync.ai/mcp-server)
- Pricing: [paidsync.ai/pricing](https://paidsync.ai/pricing)
- Category guide: [Best MCP Server for Google Ads in 2026](https://paidsync.ai/blog/best-mcp-server-for-google-ads)
- Head-to-head: [PaidSync vs official Google Ads MCP](https://paidsync.ai/vs/google-ads-mcp)
- Support: support@paidsync.ai

Built by [Ahmed Ashraf](https://paidsync.ai/about/ahmed-ashraf), Google Premier Partner.
