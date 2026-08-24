# Google Ads prompts

Paste these into Claude, ChatGPT, Gemini, Cursor, or any MCP client once PaidSync is connected. Every prompt below maps to tools that exist in the live Google Ads catalog. Each one lists the tools it touches so you can see what the assistant will actually call.

Writes are approval-gated. Where a prompt changes something, ask for the dry run first. Destructive tools also need `confirm_destructive: true`.

---

## 1. Connect and pick an account

```
Connect my Google Ads account, list every customer I can access, and set the one
named "Acme Retail US" as the active account for this session.
```

Tools: `connect_google_ads`, `list_google_ads_customers`, `set_google_ads_customer`

## 2. Manager account sweep

```
List every child account under my manager account. For each one, pull last 30 days
spend, conversions, and CPA. Rank them by wasted spend and tell me which three
accounts need attention first.
```

Tools: `list_mcc_child_accounts`, `set_mcc_context`, `get_campaign_performance`, `find_wasted_spend`

## 3. Full account audit

```
Run a full audit on the active account. Show wasted spend, low quality score
keywords, and broken conversion tracking. Then list Google's own recommendations
and tell me which to apply and which to dismiss. Finish with the change history
for the last 14 days so I can see what moved.
```

Tools: `run_account_audit`, `find_wasted_spend`, `get_quality_score_report`, `get_conversion_tracking_audit`, `list_recommendations`, `apply_recommendation`, `dismiss_recommendation`, `get_change_history`

## 4. Search terms cleanup

```
Pull the last 30 days of search terms sorted by cost. Show every term with more
than $50 spend and zero conversions. Group them into "clearly irrelevant" and
"maybe relevant". Add the clearly irrelevant ones as campaign negatives after
I confirm the list.
```

Tools: `get_search_terms_report`, `find_wasted_search_terms`, `add_campaign_negative_keywords`

## 5. N-gram waste analysis

```
Run an n-gram analysis on my search terms for the last 90 days. Show the 15 worst
two-word and three-word groups by cost per conversion. Check my existing negative
keyword lists first so you do not propose duplicates, then build the new list.
```

Tools: `get_ngram_analysis`, `list_negative_keyword_lists`, `add_negative_keywords`

## 6. Prune the dead weight

```
Find every ad group with more than $200 spend and zero conversions in the last 30
days. Do the same for keywords with more than $100 spend and zero conversions.
Show me both lists as a dry run, then pause them once I confirm.
```

Tools: `get_ad_group_performance`, `get_keyword_performance`, `pause_google_ads_ad_group`, `update_keyword`

## 7. Quality score diagnosis

```
List every keyword with a quality score below 5, with its ad group, its current
responsive search ad headlines, and its landing page. For each one tell me which
component is dragging it down: expected CTR, ad relevance, or landing page
experience.
```

Tools: `get_quality_score_report`, `list_keywords`, `list_google_ads_ads`

## 8. Build a search campaign

```
Build a search campaign for AI ad management keywords. $80 a day, Maximize
Conversions, United States only. Create one ad group, add 15 phrase match
keywords, and write two responsive search ads with 12 headlines each.
Show me the dry run of every step before you create anything.
```

Tools: `create_google_ads_campaign`, `create_google_ads_ad_group`, `add_keywords`, `create_responsive_search_ad`

## 9. Fix asset coverage

```
Check which of my active campaigns are missing sitelinks, callouts, or structured
snippets. Write the missing assets from the copy already on my site, then link
them at campaign level. Show me the copy before you create the assets.
```

Tools: `list_assets`, `create_sitelink_asset`, `create_callout_asset`, `create_structured_snippet_asset`, `link_asset_to_campaign`

## 10. Open the Performance Max black box

```
List my Performance Max campaigns. Which asset groups are rated Poor, which
search categories are eating the most budget, and which of those categories
should become account level negatives? Tell me what to change in each weak
asset group.
```

Tools: `list_pmax_campaigns`, `get_pmax_asset_group_insights`, `get_pmax_search_categories`, `get_pmax_insights`, `update_pmax_asset_group`

## 11. Where the money actually goes

```
Break my last 30 days down by device, network, geography, and hour of day. Show
me every segment where cost per conversion is more than 50% above the account
average. Then pull auction insights for the campaign with the biggest gap and
tell me who is outranking me.
```

Tools: `get_performance_by_device`, `get_performance_by_network`, `get_performance_by_geo`, `get_performance_by_time`, `get_auction_insights`

## 12. Budget pacing check

```
Show budget pacing for every campaign this month. Flag anything on track to
overspend by more than 15% or underspend by more than 25%. Propose the new
daily budgets and apply them once I approve.
```

Tools: `list_budgets`, `get_budget_pacing`, `update_campaign_budget`

## 13. Bidding strategy review

```
List my bidding strategies. For every campaign on Target CPA with fewer than 15
conversions in the last 30 days, propose a switch and explain the trade off.
Set the change up as a 50/50 experiment first, and tell me when to check results.
```

Tools: `list_bidding_strategies`, `update_campaign_bidding`, `create_portfolio_bidding_strategy`, `create_experiment`, `list_experiments`, `get_experiment_results`, `promote_experiment`

## 14. Build a high value audience

```
Create a customer match list called "High LTV 2026" from the emails in this file,
then build a similar audience from it. Attach both to my top three ad groups by
spend as observation, not targeting. Also show me which in-market segments match
my product so I can layer them next.
```

Tools: `create_customer_match_list`, `upload_customer_match_members`, `create_similar_audience`, `list_user_lists`, `list_in_market_segments`, `attach_audience_to_ad_group`

## 15. Offline conversions and diagnostics

```
Import last week's closed-won deals as offline conversions against the "Qualified
Lead" action, and import the call conversions from the same period. Then run
conversion diagnostics and tell me how many rows failed to match a click and why.
```

Tools: `list_conversion_actions`, `create_conversion_action`, `import_offline_conversions`, `import_offline_call_conversions`, `get_conversion_diagnostics`

---

## How to write your own

The pattern that works.

1. **Scope it.** Name the date range, the campaign type, and the account.
2. **Give a threshold.** "CPA above $50 with fewer than 5 conversions" beats "underperformers".
3. **Name the action.** Pause, create, import, audit, report.
4. **Add the safeguard.** "Show me the dry run first" or "confirm before applying".

Avoid vague asks like "optimize my account". The assistant has 133 Google Ads tools and no way to guess which threshold you meant.

Full tool catalog with live argument signatures comes from `paidsync_context({ include_tools: ['google_ads'] })`. Any single tool schema comes from `paidsync_tool_detail`.
