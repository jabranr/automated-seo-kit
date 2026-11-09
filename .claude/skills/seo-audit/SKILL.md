---
name: seo-audit
description: Weekly performance and technical audit of existing pages that outputs seo:refresh and seo:technical GitHub issues. Use when asked to "run seo-audit", "audit SEO performance", or by the Friday audit scheduled task.
---

# seo-audit

Output: up to `cadence.refreshes_per_week` `seo:refresh` issues, plus any `seo:technical` issues,
plus a weekly summary comment on the tracking issue.

## Data (free first)

- `get_search_console_performance`: `['page']` last_28_days vs previous 28 days (explicit dates);
  `['query','page']` last_3_months; `['date']` for the trend.
- `get_rank_tracker` / `run_rank_tracker` (if scheduled; within budget).
- `inspect_urls` for pages published 7+ days ago that have 0 impressions, and for money pages.
- Monthly or when budget allows: `run_site_audit` → `get_audit_issues`.
- Conversion data per page if `attribution.first_touch_recorded` (see seo-conversion for how).

## Diagnose → action

| Signal                                                     | Issue                                                                 |
| ---------------------------------------------------------- | --------------------------------------------------------------------- |
| High impressions, CTR below expected for its position      | `seo:refresh` — rewrite title + meta description (test 1 change)      |
| Avg position 8–20 with real impressions                    | `seo:refresh` — expand: missing subtopics/PAA, better answer, links   |
| Clicks down >30% vs previous period                        | `seo:refresh` — compare SERP now vs then; update facts, freshness     |
| Ranks but converts poorly vs its type                      | `seo:refresh` — CTA, intent match, link to money page                 |
| Two URLs ranking for the same query                        | `seo:refresh` — merge/redirect or differentiate                        |
| Not indexed 14+ days, or "crawled – not indexed"           | `seo:technical` or `seo:refresh` (thin/duplicate) — say which         |
| URL variants (slash, `#`, `?`) or app/auth pages in results | `seo:technical` — canonicals, redirects, `noindex`                     |
| Site-audit errors (broken links, missing meta, slow pages) | `seo:technical`                                                        |

Prefer refreshes on pages that already show demand. Don't refresh a page changed in the last 21
days (let Google re-crawl). Each refresh issue uses the `seo-refresh` template and a brief from the
**seo-brief** skill (refresh mode). Skip duplicates of open issues.

## Summary comment

Week-over-week clicks, impressions, CTR, top movers up/down, new pages' index status, issues opened,
credits used, and one sentence on what to try next.
