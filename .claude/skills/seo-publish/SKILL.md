---
name: seo-publish
description: After a human has merged and deployed SEO PRs, submit the new/changed URLs for indexing, verify they are live, and close out the issues. Use when asked to "run seo-publish" or by the daily publish scheduled task. Does not merge or deploy.
---

# seo-publish

1. Find PRs merged since the last run whose issues are labelled `seo:review`
   (check the tracking issue for the last-run marker).
2. For each changed URL: fetch it from `site.base_url`. Confirm HTTP 200, the canonical, the title,
   that it's in the sitemap, and not `noindex`. If not live yet (deploy pending), leave it for the
   next run.
3. `submit_indexnow_urls` with all live new/changed URLs (and the sitemap). Free/cheap; batch them.
4. `inspect_urls` (≤ 10) for the same URLs; record coverage state. Re-check in the audit after
   ~7 days.
5. If the rank tracker lacks the page's target query, `add_rank_tracking_keywords` (ask first if
   it costs credits beyond budget).
6. Label issues `seo:published`, close them, and append a line per URL to the tracking issue:
   date, URL, target query, issue/PR.
