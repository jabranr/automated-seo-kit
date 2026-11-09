---
name: seo-research
description: Weekly SEO opportunity research that outputs GitHub issues labelled seo:brief. Use when asked to "run seo-research", "find SEO opportunities", or by the weekly research scheduled task. Reads seo.config.yaml and OpenSEO; ranks by expected conversions, not clicks.
---

# seo-research

Output: up to `cadence.new_pages_per_week` new `seo:brief` issues, each a full brief.

## Preflight

- Read `seo.config.yaml`, repo AGENTS.md/CLAUDE.md, and `get_project_context` (incl. research log
  — don't re-buy research done in the last 30 days).
- Count open PRs labelled `seo:review` or `seo:drafting`. If ≥ `github.max_open_review_prs`, post a
  one-line comment on the tracking issue ("skipped: review backlog N") and stop.
- List existing open `seo:brief` issues and all published URLs (sitemap or content dir).

## Gather (free first, paid second)

1. **Search Console (free):** `get_search_console_performance`, last_3_months:
   - `['query']` with minPosition 5, maxPosition 30, minImpressions 20 → striking distance.
   - `['query','page']` → which page already ranks for each query (cannibalization + "refresh
     instead of new" candidates — hand those to audit by labelling `seo:refresh`, not here).
   - Queries with impressions but no matching page → new-page candidates.
2. **OpenSEO (paid, within budget):** `get_search_opportunities`, `get_domain_keyword_suggestions`,
   `research_keywords` for preset page types × audience pains, `find_serp_competitors` /
   `get_ranked_keywords` on 1–2 competitors for gaps. Use `get_keyword_metrics` to size.
   Log what you bought (update project context research log).
3. **Conversion weights:** `weights` in config (from seo-conversion).

## Filter

Drop candidates that: match `audience.not_who`; duplicate an existing URL's intent; need facts not
in `facts.sources`; are page types outside `content.page_types`; are navigational for another brand
(unless a comparison page is allowed).

## Score

`score = demand × winnability × intent_fit × weight(page_type) × weight(cluster)`

- demand: volume or GSC impressions (log-scaled)
- winnability: GSC position if any; else KD/competition vs our domain's strength
- intent_fit (0–1): how close the searcher is to `goal.north_star` for the preset
  (e.g. saas: "X alternative" 1.0, "how to X in Next.js" 0.8, "what is X" 0.3)

Take the top N. For each, run the **seo-brief** skill to produce the brief, then open an issue with
the `seo-brief` template, label `seo:brief`.

## Report

Comment on the tracking issue: candidates considered, chosen briefs with scores, credits used.
