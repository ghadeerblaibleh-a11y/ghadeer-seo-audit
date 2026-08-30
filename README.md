# Ghadeer SEO Audit Toolkit

A Claude Code toolkit for auditing and improving a website's SEO and GEO
(Generative Engine Optimization, meaning how well a site is set up to be
found and cited by AI tools like ChatGPT, Perplexity, and Google AI
Overviews). Everything runs through one command: `/seo`.

This README is written so that someone who doesn't code can still use the
toolkit and understand what it produces. If you're comfortable with the
technical details, the individual files in `.claude/agents/` and
`.claude/config/scoring.md` have the full specifics.

## What this is, in plain terms

You type a command like `/seo audit https://example.com` in Claude Code,
and it:

1. Fetches and inspects the actual website (or the specific thing you
   asked about, like just the images, or just the schema markup).
2. Reports back what it found, labeled by how urgent each issue is.
3. For a full audit, gives you one overall score out of 100 to help you
   prioritize what to fix first.

Every check is honest about its limits. If something couldn't be verified
(a page wouldn't load, a tool wasn't available, a piece of data wasn't
accessible), the toolkit says so in a "Not Checked" section instead of
guessing or pretending it checked something it didn't.

## Folder structure

```
.claude/
  commands/
    seo.md              <- defines the /seo command and its subcommands
  agents/
    seo-audit.md             <- runs a full audit, combines everything
    seo-technical.md         <- crawlability, security, speed, mobile
    seo-content.md           <- content quality (E-E-A-T), thin content
    seo-schema.md            <- schema.org structured data
    seo-sitemap.md           <- XML sitemap check/generation
    seo-images.md            <- image optimization
    seo-geo.md                <- AI search readiness (ChatGPT, Perplexity, etc.)
    seo-local.md              <- Google Business Profile, NAP, reviews
    seo-maps.md               <- map pack rank tracking, competitor radius
    seo-plan.md               <- strategic plan by business type
    seo-hreflang.md           <- multi-language SEO, EN/AR bilingual sites
    seo-competitor-pages.md   <- "X vs Y" and "alternatives to X" pages
    report-writer.md          <- turns findings into a bilingual client report
  config/
    scoring.md            <- the scoring model and severity definitions
README.md                 <- this file
```

- **`.claude/commands/seo.md`** is the "front door." It reads what you
  typed after `/seo`, figures out which check you want, and hands the job
  to the right specialist file below it.
- **`.claude/agents/`** contains one file per specialist. Each file is a
  detailed set of instructions for that specific type of check: what to
  look at, how to score it, and exactly how to format the results.
- **`.claude/config/scoring.md`** is the shared rulebook every specialist
  follows for scoring and for labeling how urgent an issue is. This keeps
  results consistent no matter which check you run.

## The scoring model

A full audit (`/seo audit <url>`) produces one overall score from 0 to
100. It's built from seven categories, each weighted by how much it
typically matters:

| Category         | Weight |
|-------------------|-------:|
| Content Quality   | 23%    |
| Technical SEO     | 22%    |
| On-Page SEO       | 20%    |
| Schema            | 10%    |
| Performance       | 10%    |
| AI Readiness      | 10%    |
| Images            | 5%     |

**Important: this score is a prioritization tool, not a Google ranking
metric.** It does not predict where your site will rank, and it is not a
guarantee of traffic or business results. Think of it like a health
checkup score: it tells you where to focus your attention, not what your
final grade with Google will be. Every report repeats this disclaimer so
it's never presented as more than it is.

### How issues are labeled

Every issue found gets one of four severity levels:

| Severity | What it means | When to act |
|----------|-----------------|----------------|
| **Critical** | Can block search engines (or users) from crawling, indexing, or using the site at all. | Review immediately |
| **High** | Strong evidence of a real risk to users or search visibility. | Review within 1 week |
| **Medium** | A genuine opportunity to improve, but not urgent. | Target within 1 month |
| **Low** | A nice-to-have polish item. | Add to your backlog |

### What "Not Checked" means

If the toolkit couldn't actually verify something (the site was
unreachable, a tool wasn't available, a piece of data required a
dashboard it doesn't have access to), it will list that plainly under a
"Not Checked" section rather than guessing. If you ever see a claim in a
report that isn't backed up this way, that's worth double-checking.

## How to run it

Type these commands directly in Claude Code. Replace `<url>` with the
actual web address you want to check, including `https://`.

| Command | What it does |
|---------|-----------------|
| `/seo audit <url>` | Full audit. Runs every relevant check and gives you one overall score plus a prioritized to-do list. Takes the longest, but gives the most complete picture. |
| `/seo technical <url>` | Checks whether search engines can crawl and index the site properly, security setup, mobile-friendliness, and page speed. |
| `/seo content <url>` | Reviews content quality: does it show real experience and expertise, is anything too thin or generic. |
| `/seo schema <url>` | Checks for structured data (the behind-the-scenes markup that helps search engines and AI understand your content), and can generate it if missing. |
| `/seo sitemap <url>` | Checks your XML sitemap (the file that lists your pages for search engines), or builds one if you don't have one. |
| `/seo images <url>` | Checks whether your images are optimized: descriptive alt text, efficient file formats, correct sizing. |
| `/seo geo <url>` | Checks how ready your site is to be found and quoted by AI tools like ChatGPT, Perplexity, and Google AI Overviews. |
| `/seo local <url>` | Checks local search signals: your business listing, name/address/phone consistency across the web, and reviews. |
| `/seo maps <url>` | Checks Google Maps visibility, your Business Profile, and how you compare to nearby competitors. |
| `/seo plan <type>` | Builds a strategic plan tailored to your kind of business. Choose one of: `saas`, `local-service`, `ecommerce`, `publisher`, `agency`. Works best after you've already run an audit. |
| `/seo hreflang <url>` | For sites available in more than one language. Checks that the different language versions are correctly linked, with special attention to English/Arabic sites. |
| `/seo competitor-pages` | Reviews your "X vs Y" or "alternatives to X" comparison pages (you'll be asked which page(s) to look at). |
| `/seo report` | Takes the results from your most recent check and turns them into a clean, client-ready report in both English and Arabic. Run this after any of the checks above. |

### A typical workflow

1. Run `/seo audit https://yoursite.com` to get the full picture and
   overall score.
2. Run `/seo plan <your business type>` to turn those findings into a
   step-by-step strategy.
3. Run `/seo report` to get a polished, bilingual summary you can share
   with a client, manager, or team.

You can also just run a single check (like `/seo images <url>`) any time
you only care about one area.

## What to expect in every report

No matter which command you run, the output will always include:

- A clear list of findings, each tagged with a severity level.
- An honest "Not Checked" section for anything that couldn't be verified.
- For scored checks, a reminder that the score is a prioritization aid,
  not a ranking guarantee.

If the toolkit can't reach your website at all (for example, due to a
network restriction), it will tell you that directly instead of guessing
what your site probably looks like.
