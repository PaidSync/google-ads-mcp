# Contributing

The Google Ads MCP server itself is hosted at [mcp.paidsync.ai](https://mcp.paidsync.ai/mcp) and runs as a managed service, so there is no server source in this repo. This repo is the developer entry point. The docs, the client configs in [`examples/`](./examples), the prompt library, and the tool catalog are what live here, and all of them are open to contributions.

## Report a tool bug

A tool bug is any Google Ads tool that errors, returns the wrong data, ignores an argument, or writes something different from what it previewed in the dry run.

[Open an issue](https://github.com/PaidSync/google-ads-mcp/issues/new) with:

1. **The tool name.** For example `get_search_terms_report`. Get the exact name from `paidsync_context({ include_tools: ['google_ads'] })`.
2. **The arguments you passed.** Redact the customer ID if you prefer, but keep the shape.
3. **What you expected and what you got.** Paste the error text verbatim if there was one.
4. **The client.** Claude Desktop, Claude Code, ChatGPT, Cursor, Windsurf, or another MCP client, and the date and time of the call so we can find it in the server logs.

Never paste your API key, an OAuth token, or a customer match upload into an issue. Anything with a key in it goes to support@paidsync.ai instead.

If the account is live and money is moving, email support@paidsync.ai directly rather than waiting on the issue queue.

## Request a tool

Google Ads has a large API surface and not all of it is exposed yet. Ask for what is missing.

[Open an issue](https://github.com/PaidSync/google-ads-mcp/issues/new) titled `Tool request` and cover:

1. **The job to be done.** "Bulk edit ad schedules across a campaign group", not "add scheduling support".
2. **The Google Ads API resource or report** it maps to, if you know it. A link to the Google Ads API reference page speeds this up a lot.
3. **Read or write.** If it is a write, say what the destructive path looks like so we can scope the approval gate.
4. **How often you would run it.** Daily, weekly, once per client onboarding.

Requests that name a real workflow get built first. We ship tools in batches and note them in the release notes.

## Contribute to this repo

Documentation fixes, a client config for an MCP client we do not cover, and new prompts in [`examples/prompts.md`](./examples/prompts.md) are all welcome.

1. Fork the repo.
2. Branch: `git checkout -b add-prompt-pmax-audit`
3. One topic per pull request.
4. In the pull request, say what you added and the real scenario it applies to.

We review weekly. Most pull requests get merged or get feedback within 7 days.

## Style rules

- Plain English. No "supercharge", no "unleash", no "game-changer".
- No em dashes. Use commas, periods, or rewrite the sentence.
- Prompts must be paste-ready and must only use tool names that exist in the live catalog. Check with `paidsync_context` before you submit.
- Specific beats vague. "Pause ad groups with CPA above $50 and fewer than 5 conversions in 30 days" beats "pause underperformers".
- Never claim a number the product does not hit. Tool counts and platform counts change, so link to the catalog rather than hardcoding a figure in prose.
- No claims about other people's products. Competitor tool counts, prices, and feature gaps go on [paidsync.ai/vs](https://paidsync.ai/vs/google-ads-mcp), where they carry a check date, not in this repo.

## Checklist when a count or a price changes

Some counts are still written out in the docs, so they can drift. If you change one, change all of them in the same pull request:

1. `README.md`, the opening paragraph, the "First call" section, the platform table, and the pricing table.
2. `llms-install.md`, the "What this server gives you" section.
3. Run `paidsync_context({ include_tools: '*' })` and compare the per-platform numbers against the tables before you open the pull request.
4. Pricing comes from [paidsync.ai/pricing](https://paidsync.ai/pricing). Retired plans never go back in.
5. Update the "last synced" date under the platform table.

## Out of scope

- Server source code. The MCP server is a hosted managed service.
- Pricing and plan changes. Pricing is set by the PaidSync team and lives at [paidsync.ai/pricing](https://paidsync.ai/pricing).
- Anything containing credentials.

## Contact

- Bugs and tool requests: [GitHub issues](https://github.com/PaidSync/google-ads-mcp/issues)
- Account, billing, and anything with a key in it: support@paidsync.ai
- Docs: [paidsync.ai/docs](https://paidsync.ai/docs)

## License

By contributing you agree your contribution is licensed under MIT, the same as this repo.
