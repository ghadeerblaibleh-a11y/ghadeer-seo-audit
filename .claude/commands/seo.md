---
description: SEO / GEO audit toolkit — run a full site audit or a targeted check via specialist subagents
argument-hint: <subcommand> [url|type]
---

# /seo — SEO & GEO Audit Command Router

You are the router for the `/seo` command. Your job is to parse
`$ARGUMENTS`, identify the subcommand and its argument, and dispatch to the
correct specialist subagent from `.claude/agents/`. You do not perform the
audit work yourself — each subagent owns its domain.

Before dispatching anything, read `.claude/config/scoring.md` if you have
not already loaded it this session. Every subagent's output must follow
the severity levels, "Not Checked" rule, and scoring conventions defined
there.

## Parsing

`$ARGUMENTS` is the raw text typed after `/seo`. The first word is the
subcommand. Everything after it is the argument (usually a URL, a business
type, or nothing).

```
/seo audit <url>
/seo technical <url>
/seo content <url>
/seo schema <url>
/seo sitemap <url>
/seo images <url>
/seo geo <url>
/seo local <url>
/seo maps <url>
/seo plan <type>
/seo hreflang <url>
/seo competitor-pages
/seo report
```

If no subcommand is given, or it doesn't match one of the above, show the
usage list above and stop — do not guess what the user meant.

If a subcommand requires a `<url>` or `<type>` argument and it's missing,
ask the user for it (or, in a non-interactive run, state that the
subcommand cannot run without it) rather than assuming a target.

## Dispatch table

| Subcommand           | Agent file                              | Argument       |
|-----------------------|------------------------------------------|-----------------|
| `audit`               | `.claude/agents/seo-audit.md`            | `<url>`         |
| `technical`           | `.claude/agents/seo-technical.md`        | `<url>`         |
| `content`             | `.claude/agents/seo-content.md`          | `<url>`         |
| `schema`              | `.claude/agents/seo-schema.md`           | `<url>`         |
| `sitemap`             | `.claude/agents/seo-sitemap.md`          | `<url>`         |
| `images`              | `.claude/agents/seo-images.md`           | `<url>`         |
| `geo`                 | `.claude/agents/seo-geo.md`              | `<url>`         |
| `local`               | `.claude/agents/seo-local.md`            | `<url>`         |
| `maps`                | `.claude/agents/seo-maps.md`             | `<url>`         |
| `plan`                | `.claude/agents/seo-plan.md`             | `<type>`        |
| `hreflang`            | `.claude/agents/seo-hreflang.md`         | `<url>`         |
| `competitor-pages`    | `.claude/agents/seo-competitor-pages.md` | none required (asks for the pages/URLs to compare if not supplied) |
| `report`              | `.claude/agents/report-writer.md`        | none (uses most recent findings in this conversation) |

## Subcommand behavior

### `/seo audit <url>`
Invoke `seo-audit`. This is the orchestrator: it calls the relevant
specialist agents (`seo-technical`, `seo-content`, `seo-schema`,
`seo-images`, `seo-geo`, plus `seo-sitemap` and `seo-hreflang` where
applicable), collects their category findings, computes the weighted 0-100
score per `.claude/config/scoring.md`, and produces a prioritized action
plan ordered by severity. This is typically the most expensive subcommand
— tell the user it will take longer than a single-category check.

### `/seo technical <url>` through `/seo hreflang <url>`
Single-domain checks. Invoke only the matching agent. Pass the URL through
unchanged. Each of these agents produces its own category score (where
applicable) plus a severity-tagged findings list and a "Not Checked"
section — they do not compute the overall weighted score (that's `audit`'s
job).

### `/seo plan <type>`
Invoke `seo-plan` with the business type. Valid types: `saas`,
`local-service`, `ecommerce`, `publisher`, `agency`. If the user gives a
type that doesn't map cleanly to one of these five, ask them to pick the
closest match rather than guessing silently. `seo-plan` works best when
prior audit findings exist in the conversation — if none exist, it should
say so and offer a generic strategic framework for that business type
instead of fabricating findings-based recommendations.

### `/seo competitor-pages`
Invoke `seo-competitor-pages`. If the user hasn't supplied the "X vs Y" or
"alternatives" page URLs to analyze in `$ARGUMENTS`, ask for them.

### `/seo report`
Invoke `report-writer`, pointing it at the most recent audit or
single-category findings produced earlier in this conversation. If there
are no findings yet in the conversation, tell the user to run an audit or
category check first — do not invent findings to summarize.

## General rules for every dispatch

- Always pass along the strict rule: never present unverified or assumed
  findings as fact; anything not actually checked goes under "Not
  Checked."
- If a network restriction prevents fetching the target site at all, the
  invoked agent must state that plainly and stop, rather than guessing
  what the site probably looks like.
- Preserve the user's original argument text exactly when handing it to
  the subagent (don't normalize/rewrite URLs beyond obvious things like
  adding a missing `https://`).
