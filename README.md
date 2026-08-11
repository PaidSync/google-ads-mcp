# Google Ads MCP

A Model Context Protocol (MCP) server that lets AI assistants (Claude, ChatGPT, Gemini, Cursor, Perplexity, or any MCP client) run Google Ads end to end. 136 typed tools with full write access, native MCC manager-account routing, approval-gated writes, and 12 more ad and measurement platforms in the same session.

**Hosted at [paidsync.ai](https://paidsync.ai). Free plan: 15 tasks a month, no card required.**

[![MCP](https://img.shields.io/badge/MCP-server-black?logo=anthropic&logoColor=white)](https://modelcontextprotocol.io)
[![Hosted](https://img.shields.io/badge/hosted-paidsync.ai-0077B6)](https://paidsync.ai/google-ads-mcp)
[![Free plan](https://img.shields.io/badge/free%20plan-15%20tasks%2Fmo-7CB518)](https://paidsync.ai/signup?plan=free)
[![Google Partner](https://img.shields.io/badge/Google-Premier%20Partner-4285F4?logo=google)](https://www.google.com/partners/agency?id=7427374971)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

## What this is

PaidSync's Google Ads MCP surface, exposed to any AI assistant that speaks the Model Context Protocol. The full product page lives at [paidsync.ai/google-ads-mcp](https://paidsync.ai/google-ads-mcp). This repository is the developer entry point: install commands, tool catalog, and links to the docs.

Three properties that set this Google Ads MCP apart from every other option currently listed on mcpservers.org, mcp.so, and the AI Overview citations:

1. **136 Google Ads tools with full write access.** Not a reporting-only reader. Campaigns, ad groups, keywords, negatives, Performance Max asset groups, bidding strategies, portfolio bidding, audiences, offline conversion imports, experiments, drafts, recommendations, and change history. Every write is a real Google Ads API call.
2. **Native MCC (My Client Center) manager-account routing.** One PaidSync connection covers every child account under the manager. The assistant switches customer context per task. Google's official MCP reference exposes the API but leaves the routing UX to whoever extends it; other community servers are single-account only.
3. **Approval-gated writes with dry-run previews.** Every mutation is held for your confirmation in chat. Most write tools accept `dry_run` to return the exact planned change before touching the account. Destructive operations require an explicit second confirmation.

## Getting started (hosted, recommended)

The fastest path is to connect the hosted server. No install, no OAuth apps, no engineering setup.

### Claude Desktop

Add this to your Claude Desktop MCP configuration:

```json
{
  "mcpServers": {
    "paidsync": {
      "url": "https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY"
    }
  }
}
```

Get your API key at [paidsync.ai/signup](https://paidsync.ai/signup?plan=free). Free plan is 15 tasks a month, no card required.

### claude.ai (web) and Claude Code

Go to [claude.ai/settings/integrations](https://claude.ai/settings/integrations) and add a custom MCP integration with URL `https://mcp.paidsync.ai/mcp?key=YOUR_API_KEY`. Same URL works in Claude Code via `claude mcp add paidsync ...`.

### ChatGPT

Add PaidSync as a custom MCP connector in Settings then Beta features then Model Context Protocol. Use the same URL as above.

### Gemini, Cursor, Windsurf, Perplexity, any MCP client

Same URL, same API key. Setup guides:
- [Connect Google Ads to Claude](https://paidsync.ai/blog/connect-google-ads-to-claude)
- [Connect Google Ads to ChatGPT](https://paidsync.ai/blog/connect-google-ads-to-chatgpt)
- [Connect Google Ads to Gemini](https://paidsync.ai/blog/connect-google-ads-to-gemini)
- [Connect Google Ads to Perplexity](https://paidsync.ai/blog/connect-google-ads-to-perplexity)
- [Connect Google Ads to Copilot](https://paidsync.ai/blog/connect-google-ads-to-copilot)

## First call

Once connected, PaidSync exposes two meta-tools that keep client context small even with a 430-tool catalog:

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

## Pricing

| Plan | Price | Tasks | Notes |
|---|---|---|---|
| Free | $0/mo | 15 | No card required, all 13 platforms |
| Pro | From $99/mo | 600 to 4,000 | Solo operators and small teams |
| Team | From $249/mo | 6,000 shared | 5 seats included, MCC-friendly |
| Done For You | Custom | Managed | PaidSync runs the accounts |

Every plan includes all 13 platforms and every MCC-linked child account. No per-account or per-platform charges. Every tool call counts as one task.

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

## Compared with other Google Ads MCP options

| | PaidSync | Google official MCP | cohnen/mcp-google-ads | AdKit |
|---|---|---|---|---|
| Google Ads tools | 136 | API surface (build it) | Reporting-focused subset | Subset of 23 total |
| Write access | Full, gated | You implement it | Read-focused | Yes, ungated |
| MCC manager-account routing | Yes, native | API supports it, no UX | Single account | Not listed |
| Approval gates on writes | Yes | You implement it | Not applicable | No, applies immediately |
| Hosted vs self-hosted | Hosted | Self-hosted | Self-hosted | Hosted |
| Platform scope in same session | 13 platforms | Google Ads only | Google Ads only | Google, Meta, LinkedIn |
| Google Partner | Google Premier (top 3% globally) | Google, first party | Not applicable | Not listed |
| Price | Free $0 to $99+/mo | Free, API quota | Free, self-hosted | $29 to $49/year |

Full comparison at [paidsync.ai/vs/google-ads-mcp](https://paidsync.ai/vs/google-ads-mcp).

## License

MIT. See [LICENSE](./LICENSE).

## Links

- Product page: [paidsync.ai/google-ads-mcp](https://paidsync.ai/google-ads-mcp)
- MCP server overview: [paidsync.ai/mcp-server](https://paidsync.ai/mcp-server)
- Pricing: [paidsync.ai/pricing](https://paidsync.ai/pricing)
- Category guide: [Best MCP Server for Google Ads in 2026](https://paidsync.ai/blog/best-mcp-server-for-google-ads)
- Head-to-head: [PaidSync vs official Google Ads MCP](https://paidsync.ai/vs/google-ads-mcp)
- Support: support@paidsync.ai

Built by [Ahmed Ashraf](https://paidsync.ai/about/ahmed-ashraf), Google Premier Partner (top 3% globally), $500M+ in managed ad spend.
