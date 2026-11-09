---
name: seo-qa
description: Independent QA review of draft SEO PRs (issue labelled seo:drafting). Passes them to human review or sends them back to the writer. Use when asked to "run seo-qa", "QA the SEO PRs", or by the QA scheduled task. Never merges.
---

# seo-qa

Act as a strict editor who did not write the page. Review each open PR whose issue is labelled
`seo:drafting`.

## Checks (all must pass)

**Deterministic** — run `node scripts/seo-lint.mjs {build.output_dir} --config seo.config.yaml --only {url}`
after building. Any error = fail.

**Facts** — every product claim traces to a file in `facts.sources` or a cited primary URL.
Prices, limits, features, hours, areas match the source *today*. Nothing in `facts.forbidden_claims`.
Code samples run against the real API/format (ids, endpoints, field names). Any doubt = fail.

**Intent & value** — the first 100 words answer the target query. The page matches the brief's
intent and type. The unique value is actually there. It's better than the top results on at least
one clear dimension. Thin or generic = fail.

**Reader** — reading level ≤ `audience.reading_level_max_grade`; no banned phrases; sounds like
`voice`; no filler intros, no repeated points, no "AI tone" (hedging stacks, triplet lists
everywhere, empty summaries).

**SEO** — title/description per brief; one H1; logical H2/H3; canonical in the site's form;
schema valid for the type; internal links present (incl. `must_link_to` where relevant) and
inbound links added; no cannibalization with another page (check GSC `['query','page']` for the
target query).

**Conversion** — CTA present, contextual, matches `goal.preset`, links resolve.

**Brand & a11y** — repo components/tokens; alt text; images real and relevant; screenshot the
page (Playwright against preview, or the PR preview URL) and look at it at mobile + desktop width.

**Repo rules** — tests/lint/typecheck pass; required tests added; nothing generated or secret
committed.

## Outcome

- **Pass:** mark PR ready for review, label issue `seo:review`, comment a short checklist summary.
- **Fail:** review comment listing each failure with the exact fix. Count QA rounds (comments
  starting `SEO-QA round`). Round < `cadence.qa_max_rounds` → label `seo:qa-failed`. Otherwise
  → `seo:needs-human` with a summary of what keeps failing.
