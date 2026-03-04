# Ditto for Content Marketing — Claude Code Skill

Generate research-backed blog articles, infographics, social threads,
and more from a single research study using [Ditto](https://askditto.io)'s
synthetic persona platform. Directly from your terminal via Claude Code.

## What This Skill Does

When installed, Claude Code automatically loads this skill whenever you
ask it to generate research-backed content, create blog articles from
studies, or build a content marketing pipeline.

Claude will:
- Design studies with a narrative arc question framework
- Run studies that produce 70 data points (10 personas x 7 questions)
- Extract content building blocks (headline finding, quotes, pain themes)
- Generate 7 content formats with SEO/GEO optimisation

## The Content Quality Test

**If you could write the article without the study data and it would read
the same, it is NOT research-backed.**

## One Study, Seven Content Formats

1. **Research Blog Article** (1,000-2,500 words, SEO/GEO optimised)
2. **Infographic** (key findings visualised)
3. **Social Media Thread** (3-5 posts)
4. **Email Outreach** (personalised per recipient)
5. **Executive Summary** (1-page, PDF-ready)
6. **Sales Leave-Behind** (for prospect sharing)
7. **Press Release** (if newsworthy)

## Installation

```bash
# For a specific project
cp -r ditto-content-marketing /path/to/your/project/.claude/skills/

# For all your projects (personal skills)
cp -r ditto-content-marketing ~/.claude/skills/
```

## Setup

Get a free Ditto API key (no credit card required):

```bash
curl -sL https://app.askditto.io/scripts/free-tier-auth.sh | bash
```

Set it as an environment variable:

```bash
export DITTO_API_KEY="rk_free_YOUR_KEY_HERE"
```

## Usage

```
"Write a research-backed blog article about how US consumers
feel about AI-powered customer service. 10 personas, SEO optimised."

"Generate a social media thread from our latest Ditto study
with the 3 most quotable findings."

"Set up a content calendar: one research article per week
across 4 different topics."
```

Or invoke directly:

```
/ditto-content-marketing "research article on [topic]"
```

## What's Included

| File | Purpose |
|------|---------|
| `SKILL.md` | Main skill — narrative arc framework, SEO/GEO, content calendar |

## About Ditto

Ditto maintains 300,000+ AI-powered synthetic personas calibrated to
census data across USA, UK, Germany, and Canada. Capacity: 20-40
research-backed articles per month at 45-90 minutes each. Traditional:
2-4 articles/month at $2,000-10,000 each.

## Links

- **Ditto:** https://askditto.io
- **Full API Guide:** https://askditto.io/claude-code-guide
- **General Research Skill:** https://github.com/Ask-Ditto/ditto-product-research-skill

## License

MIT