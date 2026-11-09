---
name: seo-setup
description: One-off onboarding of a repo into the Automated SEO Kit. Use when asked to "run seo-setup", "set up automated SEO", "set up agentic SEO", or "onboard this site to the SEO loop". Fills seo.config.yaml, syncs OpenSEO project context, creates GitHub labels and a rank tracker, and checks conversion tracking.
---

# seo-setup

Goal: leave the repo ready for the weekly loop. Ask the user for anything you cannot infer.
Never spend OpenSEO credits without showing the estimate and getting a yes.

## Steps

1. **Read rules.** Read the repo's AGENTS.md / CLAUDE.md and README. They override this kit.

2. **Find the OpenSEO project.** `list_projects` → match `site.domain`. If none, stop and ask
   the user to create it (needs the domain). Then `list_search_console_sites` for that project and
   confirm one property has `isSelected: true` and `requiresReconnect: false`. Report the bound
   property. If not bound, ask before calling `set_search_console_site`.

3. **Fill `seo.config.yaml`** (copy from `seo.config.example.yaml` if missing). Infer from code:
   content dir + format + how a new page is added (read 2 existing pages and the router/sitemap
   code), url pattern, trailing-slash form, build/test/lint commands, output dir, money pages,
   fact sources. Ask the user for: `goal.preset`, `north_star`, conversion event names,
   audience/not_who, voice, competitors, weekly credit budget. Validate your answers with them.

4. **Sync OpenSEO context.** `get_project_context`; fill missing sections with
   `update_project_context`: business_overview, current_goal (north star + 90-day target),
   positioning, writing_preferences (voice + banned phrases + reading level), competitors, key
   pages (money pages). Optionally `research_project_website` (check cost first).

5. **Baseline.** Free calls only:
   - `get_search_console_performance` last_3_months by `page` and by `query` → note top pages,
     striking-distance queries (pos 5–20, ≥50 impressions; lower threshold for new sites).
   - Look for URL-variant duplicates (trailing slash, `#fragment`, `?query`), and app/auth pages
     (login, dashboard) getting impressions → open `seo:technical` issues.
   - `inspect_urls` on home + top 5 money pages.
   Write a short baseline comment into a pinned tracking issue "SEO loop — tracking".

6. **Labels.** Create labels from `.github/seo-labels.json` (skip existing).

7. **Rank tracker.** Propose 20–40 keywords (from GSC queries + preset page types + competitors).
   `estimate_rank_tracker_cost` → ask → `create_rank_tracker` + `add_rank_tracking_keywords`.
   Save the id in config.

8. **Conversion tracking check.** Find where `goal.conversion_events` are recorded (analytics code,
   DB tables, API). Check whether first landing path + referrer are stored on the conversion.
   If not: open a `seo:technical` issue describing first-touch attribution for this stack
   (capture on first pageview in first-party storage, attach at conversion, query for page →
   conversion). Respect repo rules (feature flags, tests). Set `attribution.first_touch_recorded`.

9. **CI.** Suggest adding `node scripts/seo-lint.mjs <output_dir> --config seo.config.yaml` to CI.
   Don't commit or push unless the repo rules and the user allow it.

10. **Report** what was set up, what's blocked, and the proposed scheduled tasks (README →
    Schedules) with times. Create them only when the user says yes.
