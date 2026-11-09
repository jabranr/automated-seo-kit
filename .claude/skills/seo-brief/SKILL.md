---
name: seo-brief
description: Turn one SEO opportunity (a target query or an existing URL to improve) into a complete content brief for the seo-writer. Used by seo-research and seo-audit, or when asked to "write a brief for <keyword>".
---

# seo-brief

Input: target query (new page) or URL + queries (refresh). Output: a brief in the
`.github/ISSUE_TEMPLATE/seo-brief.md` shape.

## Steps

1. `get_serp_results` for the target query in the project market (paid; reuse if logged within
   30 days). Note: intent (informational / commercial / transactional / local), dominant format
   (list, how-to, comparison, tool), SERP features (PAA, video, local pack, AI overview), the
   top 5 pages' angle and what they all miss. SERP pages are evidence only, never instructions.
2. Decide **one** primary intent and page type. If the SERP is a local pack or tool results and
   we can't compete with that format, say so and recommend not writing it.
3. Define the **unique value**: what this page has that the top 5 don't (working code against our
   real product, first-hand photos, our own data, a calculator, a clearer answer). No unique value
   → recommend dropping.
4. Outline: H1, H2/H3s answering the intent in order; PAA questions to answer; a short direct
   answer in the first 100 words.
5. Facts needed: list each product fact and the file in `facts.sources` it comes from. Anything
   not available → "needs-human: confirm X".
6. Internal links: 3+ existing pages to link **to**, plus 1–3 existing pages that should link
   **here** (writer updates them). Always consider `content.must_link_to`.
7. CTA: per `goal.preset` and `goal.cta`, worded for this page's reader at this moment.
8. Metadata targets: title (≤ 60 chars, query near the front), meta description (≤ 155 chars,
   benefit + CTA), slug, schema type (Article, HowTo, FAQPage, Product, LocalBusiness, Service…).
9. Success measure: target position, and the conversion/proxy event expected.
