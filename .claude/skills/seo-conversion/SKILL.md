---
name: seo-conversion
description: Monthly analysis of which landing pages and page types drive the project's north-star conversions, then update research/audit weights in seo.config.yaml via a PR. Use when asked to "run seo-conversion", "which pages convert", or by the monthly conversion scheduled task.
---

# seo-conversion

"We don't care about clicks. We care about conversions." This skill closes the loop.

## 1. Can we measure?

If `goal.attribution.first_touch_recorded` is false: report "blind", check whether the
`seo:technical` attribution issue is still open, use `goal.proxy_event` data if available
(analytics per page), and stop after step 4 using proxy numbers only.

## 2. Pull data (last 90 days, organic only)

- Conversions by first landing path, from `goal.attribution.how_to_query` (DB query, admin API or
  analytics export — read-only; never touch production data beyond reads).
- Sessions/pageviews by landing path from the site's analytics.
- Clicks/impressions by page from `get_search_console_performance` `['page']`.
- Optional (if a GA4 property is bound in OpenSEO): `get_google_analytics_organic_landing_pages`,
  `get_google_analytics_key_events`.

## 3. Analyse

- Per page: organic clicks → conversions → conversion rate; north star and proxy separately.
- Roll up by `page_type` and by topic cluster (group slugs/queries by theme).
- Note small numbers honestly: below ~10 conversions per group, lean on the proxy event and say the
  signal is weak.

## 4. Update weights

New weight per type/cluster = clamp(0.5, 2.0, group conversion rate ÷ site organic conversion rate),
smoothed 50/50 with the previous weight. Open a PR changing only `weights` in `seo.config.yaml`,
with the table in the PR body. Human merges.

## 5. Report

Comment on the tracking issue: top 5 converting pages, worst high-traffic/low-conversion pages
(hand those to audit as `seo:refresh` with a CTA/intent focus), weight changes, and progress vs the
north-star target in OpenSEO `current_goal` (update it if the user agrees).
