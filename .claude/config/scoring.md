# SEO Health Score — Scoring Model

This document defines the single scoring model used by every agent in this
toolkit (`seo-audit`, and any subagent that reports a category score). All
agents must reference this file rather than inventing their own weights or
severity language, so that scores are comparable across runs and across
subcommands.

## What this score is (and is not)

- The 0-100 score is a **prioritization heuristic**. It exists to help a
  human decide what to fix first, given limited time.
- It is **not** a Google (or Bing, or any AI engine) ranking metric.
- It is **not** a prediction or guarantee of traffic, rankings, or revenue
  outcomes.
- It is a snapshot based only on what was actually checked during the audit.
  Anything not checked is excluded from scoring and disclosed separately
  (see "Not Checked" rule below).
- Two sites with the same score can have very different underlying issues.
  Always read the findings, not just the number.

Every report that surfaces this score must restate this disclaimer in plain
language, not just link to this file.

## Category weights

The overall score is a weighted sum of seven category scores, each scored
independently on a 0-100 scale before weighting.

| Category         | Weight | Primary agent(s)                  |
|-------------------|-------:|------------------------------------|
| Content Quality   | 23%    | seo-content                        |
| Technical SEO     | 22%    | seo-technical                      |
| On-Page SEO       | 20%    | seo-technical, seo-content         |
| Schema            | 10%    | seo-schema                         |
| Performance       | 10%    | seo-technical                      |
| AI Readiness      | 10%    | seo-geo                            |
| Images            | 5%     | seo-images                         |

Weights sum to 100%. The overall score is:

```
Overall = (Content Quality x 0.23)
        + (Technical SEO   x 0.22)
        + (On-Page SEO     x 0.20)
        + (Schema          x 0.10)
        + (Performance     x 0.10)
        + (AI Readiness    x 0.10)
        + (Images          x 0.05)
```

### If a category could not be assessed

If an entire category could not be checked (for example, a network
restriction blocked fetching the site, or performance tooling was
unavailable), do not silently assign it a 0 or guess a value. Instead:

1. Exclude that category from the weighted formula.
2. Re-normalize the remaining weights proportionally so they still sum to
   100%, and show the math.
3. State explicitly in the report: "Overall score is based on N of 7
   categories because [category] could not be checked. See 'Not Checked'."

A partial score must always be labeled as partial. Never present it as a
full 100%-coverage score.

## Category scoring guidance

Each category agent scores 0-100 based on the proportion and severity of
issues found relative to what a healthy version of that category looks
like. As a general anchor:

- 90-100: No Critical or High issues; only Low-severity polish items remain.
- 70-89: No Critical issues; some High-severity items outstanding.
- 40-69: At least one High-severity issue, or many Medium-severity issues.
- 0-39: One or more Critical issues, or High-severity issues across most
  pages/areas checked.

Agents should show their work: list the specific findings that drove the
score up or down rather than presenting the number as a black box.

## Severity levels

All findings across all agents must be tagged with exactly one of these
four severity levels, using this exact language:

| Severity  | Definition                                                              | Action window       |
|-----------|--------------------------------------------------------------------------|----------------------|
| Critical  | Can block crawling, indexing, or core user flows.                       | Review immediately   |
| High      | Strong evidence of user or search risk.                                 | Review within 1 week |
| Medium    | Optimization opportunity.                                                | Target within 1 month|
| Low       | Nice to have.                                                            | Add to backlog       |

Guidelines:

- Never invent a fifth severity level or rename these four.
- Every finding needs a severity, a one-line reason, and the action window.
- When in doubt between two adjacent severities, choose the lower
  (less urgent) one unless there is clear evidence of user or crawl impact.

## The "Not Checked" rule (applies to every agent)

This is a hard rule for the entire toolkit, not just scoring:

- Never present an unverified or assumed finding as fact.
- If something was not actually fetched, tested, or inspected, it must be
  listed under a **Not Checked** section in the output, not folded into
  the findings or the score.
- If a network restriction, authentication wall, robots.txt block, or tool
  limitation prevented checking something, state that plainly ("Could not
  fetch /pricing — request timed out" or "No API access to Google Search
  Console, so impressions/clicks were not verified") instead of guessing
  or inferring likely results.
- Findings inferred from indirect evidence (e.g., "the homepage has no
  visible reviews, so review markup is likely absent") must be labeled as
  an inference, not a confirmed finding, and should still prefer to be
  listed under "Not Checked" or "Needs Verification" if not directly
  confirmed.

## Output contract for scores

Any agent that reports a score must include:

1. The category score(s) and how they were derived (which findings drove
   them).
2. The severity table for findings in that category.
3. A "Not Checked" section (present even if empty — state "Nothing
   excluded; all planned checks completed.").
4. The scoring disclaimer from "What this score is" above, in short form,
   e.g.: "This score is a prioritization aid, not a ranking guarantee."
