---
name: seo-writer
description: Write or rewrite one page from a GitHub issue labelled seo:brief, seo:refresh or seo:qa-failed, on a branch, and open or update a draft PR. Use when asked to "run seo-writer", "write the next SEO post", or by the writer scheduled task. Never merges.
---

# seo-writer

## Pick work (one item per run)

Priority: `seo:qa-failed` (oldest first) → `seo:refresh` → `seo:brief` (highest score). Skip issues
labelled `seo:needs-human`. Read the issue, every comment, and (for qa-failed) the QA review on the
PR.

## Prepare

- Read `seo.config.yaml`, repo AGENTS.md/CLAUDE.md (tests, flags, lint, commit rules win),
  `get_project_context` writing preferences.
- Read 2 existing pages of the same type to match structure, components and tone.
- Read every file the brief cites as a fact source. If a needed fact isn't there, don't guess:
  comment on the issue, label `seo:needs-human`, stop.

## Write

- Branch: `{branch_prefix}{issue}-{slug}` from `github.base_branch` (reuse it for qa-failed).
- Follow `content.new_page_steps` exactly. Use the repo's components/design tokens; no inline styles
  or new UI unless the repo rules allow it.
- Answer the query in the first 100 words. One idea per paragraph, short sentences, reading level ≤
  `audience.reading_level_max_grade`. No `voice.banned_phrases`. Use `voice.brand_name_rule`.
- Unique value from the brief must be present (real working code tested against the real API,
  real images per `content.images`, real data with source). Never fabricate screenshots, quotes,
  testimonials, stats or customer names.
- Links: the brief's internal links; add the inbound links from the listed existing pages; external
  links only to primary sources.
- CTA per brief. Metadata (title, description, canonical in the site's trailing-slash form), schema
  per brief, alt text on every image.
- Refreshes: keep the URL. Improve what the audit asked for; don't rewrite what already ranks unless
  told to. Note the change in the PR.

## Verify

Run `build.build`, `build.test`, `build.lint`, and
`node scripts/seo-lint.mjs {build.output_dir} --config seo.config.yaml --only {url}`. Fix failures.
Add/update tests the repo rules require (e.g. content catalog specs).

## PR

Open (or update) a **draft** PR "SEO: {title}" linking `Closes #{issue}`, with: target query,
intent, unique value, fact sources used, links added, lint output. Label the issue
`seo:drafting` (remove `seo:brief`/`seo:refresh`/`seo:qa-failed`). Commit messages follow repo rules.
