---
name: ditto-content-marketing
description: >
  Generate research-backed content at scale using Ditto's synthetic persona
  platform (300K+ AI personas, 92% overlap with real focus groups). One
  10-persona, 7-question study produces 7 content formats: blog article,
  infographic, social thread, email outreach, executive summary, sales
  leave-behind, and press release. Covers narrative arc question design,
  SEO/GEO optimisation, content building block extraction, and an always-on
  content calendar (20-40 articles/month). Designed for content marketers,
  content directors, SEO specialists, and marketing managers scaling
  research-backed content. Use when the user mentions content marketing,
  research blog, article generation, social media content, content pipeline,
  SEO content, or research-backed content.
allowed-tools: Bash(curl *), Bash(python3 *), Read, Grep, WebFetch
argument-hint: "[topic, product, or content brief]"
---

# Ditto for Content Marketing

Generate research-backed blog articles, infographics, social threads, and
more from a single Ditto study — at a pace of 20-40 articles per month.
Directly from the terminal.

**Full documentation:** https://askditto.io/claude-code-guide

## What Ditto Does

Ditto maintains 300,000+ AI-powered synthetic personas calibrated to census
data across USA, UK, Germany, and Canada. You ask them open-ended questions
and get qualitative responses with the specificity of real interviews.

- **92% overlap** with traditional focus groups (EY Americas validation)
- Every study produces **70 data points** (10 personas x 7 questions)
- One study → 7 content formats in **~60 minutes**
- Traditional research-backed content: 2-4 articles/month, $2,000-10,000 each

## The Content Quality Test

**If you could write the article without the study data and it would read
the same, it is NOT research-backed.**

Every paragraph must contain a finding, quote, or data point from the
specific study. Generic commentary with a study link appended is not content
marketing — it is filler.

## Quick Start (Free Tier)

Get a free API key — no credit card, no sales call:

```bash
curl -sL https://app.askditto.io/scripts/free-tier-auth.sh | bash
```

Free keys (`rk_free_`): ~12 shared personas, no demographic filtering.
Paid keys (`rk_live_`): custom groups, demographic filtering, unlimited studies.

## API Essentials

**Base URL:** `https://app.askditto.io`
**Auth header:** `Authorization: Bearer YOUR_API_KEY`
**Content-Type:** `application/json`

## One Study, Seven Content Formats

```
One 10-Persona Study (~12 min)
    ├─ 1. Research Blog Article (1,000-2,500 words, SEO/GEO optimised)
    ├─ 2. Infographic (key findings visualised)
    ├─ 3. Social Media Thread (3-5 posts)
    ├─ 4. Email Outreach (personalised per recipient)
    ├─ 5. Executive Summary (1-page, PDF-ready)
    ├─ 6. Sales Leave-Behind (for prospect sharing)
    └─ 7. Press Release (if newsworthy)

Total: ~60 min from zero to complete content kit
Traditional: 2-4 weeks, $5,000-15,000
```

## The Content Marketing Workflow (6 Steps)

### Step 1: Recruit Your Audience Panel

Match the panel to the content's target reader:

```bash
curl -s -X POST "https://app.askditto.io/v1/research-groups/recruit" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "US Coffee Consumers 25-55",
    "group_size": 10,
    "filters": {
      "country": "USA",
      "age_min": 25,
      "age_max": 55
    }
  }'
```

Save the `uuid`. Use `group_size` (not `size`), group `uuid` (not `id`).

### Step 2: Create Study

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Consumer Perceptions of [Topic]",
    "objective": "Generate research-backed content on [topic] with quotable insights",
    "research_group_uuid": "UUID_FROM_STEP_1"
  }'
```

Response nests under `data.study` — access via `response["study"]["id"]`.

### Step 3: Ask the 7 Content Questions

Ask one at a time. Poll to completion between each (see Polling Strategy).
**Do NOT batch questions** — batching produces shallow, disconnected answers.

### Step 4: Poll Until Complete

```bash
curl -s "https://app.askditto.io/v1/jobs/JOB_ID" \
  -H "Authorization: Bearer $DITTO_API_KEY"
```

Wait **45-50 seconds** before first poll, then **20 seconds** intervals.
Poll **ONE** job_id as proxy.

### Step 5: Complete the Study

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/complete" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"force": false}'
```

### Step 6: Extract Content Building Blocks + Get Share Link

```bash
curl -s "https://app.askditto.io/v1/research-studies/STUDY_ID/questions" \
  -H "Authorization: Bearer $DITTO_API_KEY"
```

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/share" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"enabled": true}'
```

**UTM tracking (mandatory):**
- Cold email share links: `?utm_source=ce`
- Blog article share links: `?utm_source=blog`

## The Content Question Framework (7 Questions)

Questions follow a **narrative arc** — not an investigative sequence. The
arc produces content that reads as a story, not a survey report.

### The Narrative Arc

```
Q1-Q2 = "Before" (the problem state)
Q3    = "Turning point" (the possibility of change)
Q4-Q6 = "Substance" (evidence, comparison, evaluation)
Q7    = "Payoff" (unfiltered truth, strongest quotes)
```

**Q1 — First Impressions** (opening hook material)
> "When you think about [product/category], what's the first thing that
> comes to mind? What's your honest impression?"

Extract: Opening hook, first-impression reactions, headline candidates.

**Q2 — Pain Stories** (relatable anecdotes)
> "What's the biggest frustration or unmet need you have with [category]?
> Tell me about a specific experience."

Extract: Pain point stories for article body, relatable anecdotes.

**Q3 — The Turning Point** (possibility of change)
> "If a product could [your value proposition], how would that change things
> for you? What excites you? What makes you sceptical?"

Extract: Excitement = opportunity content. Scepticism = objection content.

**Q4 — Current State** (competitive landscape)
> "How do you currently solve this problem? What tools, services, or
> workarounds do you use? What do you spend on it?"

Extract: Competitive landscape content, cost comparisons, workaround stories.

**Q5 — Product Reaction** (direct feedback)
> "Looking at [product/website/feature], what stands out? What's confusing?
> What would make you want to try it?"

Extract: Product reaction quotes, UX feedback, conversion triggers.

**Q6 — Decision Criteria** (comparative content)
> "If you were choosing between [product] and [top alternative], what would
> tip the decision? What evidence would you need?"

Extract: Competitive content, proof point needs, decision criteria.

**Q7 — Unfiltered Truth** (the payoff — strongest quotes)
> "Is there anything about [category] that you feel companies just don't
> understand? What do you wish they knew?"

Extract: Strongest quotes for social media. Unfiltered frustration for
positioning. Best content hooks.

## Content Building Blocks

Extract from every study:

| Block | Description | Source |
|-------|-------------|--------|
| **Headline Finding** | Single most surprising data point | Cross-question pattern |
| **Quotable Reactions** | 3-5 vivid, specific quotes with demographics | Q1, Q3, Q7 |
| **Pain Point Themes** | Recurring frustrations ranked by frequency | Q2, Q4 |
| **Competitive Landscape** | What alternatives exist and why they fall short | Q4, Q6 |
| **Surprise Insight** | Something that contradicts conventional wisdom | Any question |
| **Data Points** | Specific numbers ("7 of 10 said...", "$X spent") | Frequency counts |

## Blog Article Structure

7 sections, 1,000-2,500 words:

1. **Hook** (50-100 words) — Lead with the headline finding
2. **Context** (100-150 words) — Why this topic matters now
3. **Who We Asked** (100-150 words) — Panel demographics, methodology
4. **What We Asked** (50-100 words) — Question themes, approach
5. **Key Findings** (400-800 words) — **THE CORE.** 3-5 findings, each with
   data point + quote + implication. This is where the study data lives.
6. **Implications** (150-200 words) — What this means for the industry
7. **Conclusion + CTA** (100-150 words) — Summary + link to live study

## SEO Optimisation

| Element | Requirement |
|---------|------------|
| Title | Under 60 characters |
| seoDescription | 150-160 characters |
| Headings | H2/H3 with keywords |
| Word count | 1,000+ minimum |
| Internal links | Minimum 2 |
| Image alt text | Descriptive, keyword-rich |

## GEO Optimisation (Generative Engine Optimisation)

Optimise for AI-powered search citation:

| Field | Format | Rules |
|-------|--------|-------|
| `llmSummary` | 200-400 words, factual prose | NO marketing language, no superlatives, no CTAs. Write as for an encyclopaedia. |
| `keyTakeaways` | 4-6 standalone facts | Each must be citable without context |
| `primaryClaim` | One sentence | Core finding in plain language |
| `quotableInsights` | 3-5 findings | Formatted for AI citation |
| `featuredForLLMs` | boolean | Priority indexing flag |

## Always-On Content Calendar

| Week | Activity | Output |
|------|----------|--------|
| **Week 1** | Run study + write blog article + generate infographic | Article + infographic live |
| **Week 2** | Social thread + email outreach | Distribution |
| **Week 3** | Executive summary + sales leave-behind | Internal assets |
| **Week 4** | Press release (if newsworthy) + next study planning | PR + pipeline |

**Capacity:** 20-40 articles per month at 45-90 minutes each.

## Media Attachments

Upload product screenshots, competitor pages, or ad creative for Q5:

```bash
curl -s -X POST "https://app.askditto.io/v1/media-assets" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "data_url": "https://example.com/landing-page.png",
    "filename": "landing-page.png",
    "mime": "image/png"
  }'
```

Pass as `attachments` when asking Q5 for richer product reactions.
Allowed types: PNG, JPEG, GIF, WEBP, PDF.

## Complete API Reference

### Research Groups

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-groups/recruit` | Recruit audience panel |
| `POST` | `/v1/research-groups/create` | Create from agent IDs |
| `POST` | `/v1/research-groups/interview` | AI-assisted recruitment |
| `GET` | `/v1/research-groups` | List groups |
| `GET` | `/v1/research-groups/{id}` | Get group details |
| `POST` | `/v1/research-groups/{id}/update` | Update group |
| `DELETE` | `/v1/research-groups/{id}` | Archive group |
| `POST` | `/v1/research-groups/{id}/agents/add` | Add agents |
| `POST` | `/v1/research-groups/{id}/agents/remove` | Remove agents |
| `POST` | `/v1/research-groups/{uuid}/append` | Recruit more into group |

**⚠️** `append` uses `group_uuid` (not `group_id`).

### Research Studies

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies` | Create study |
| `GET` | `/v1/research-studies` | List studies |
| `GET` | `/v1/research-studies/{id}` | Get study details |
| `POST` | `/v1/research-studies/{id}/complete` | Trigger AI analysis |
| `POST` | `/v1/research-studies/{id}/agents/remove` | Remove agents |

### Questions

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies/{id}/questions` | Ask question (returns job_ids) |
| `GET` | `/v1/research-studies/{id}/questions` | Get all Q&A data |
| `POST` | `/v1/research-agents/{id}/questions` | Quick question to one persona |
| `POST` | `/v1/research-groups/{id}/questions` | Quick question to group |

### Jobs, Sharing, Media

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `GET` | `/v1/jobs/{job_id}` | Poll async job status |
| `POST` | `/v1/research-studies/{id}/share` | Enable sharing |
| `GET` | `/v1/research-studies/{id}/share` | Check share state |
| `POST` | `/v1/media-assets` | Upload image/PDF |

### Agents, NL Requests, Zeitgeist, Free Tier

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `GET` | `/v1/agents/find` | Find matching persona |
| `GET` | `/v1/agents/search` | Search personas |
| `POST` | `/v1/research-study-requests` | NL study request |
| `POST` | `/v1/research-group-requests` | NL group request |
| `GET` | `/v1/research-group-requests/{id}` | Group request status |
| `POST` | `/v1/zeitgeist/surveys/create` | Quick survey |
| `GET` | `/v1/zeitgeist/surveys/{id}/results` | Survey results |
| `DELETE` | `/v1/zeitgeist/surveys/{id}` | Delete survey |
| `POST` | `/v1/free/questions` | Free-tier question |

## Demographic Filters

| Filter | Type | Examples | Notes |
|--------|------|----------|-------|
| `country` | string | `"USA"`, `"UK"`, `"Canada"`, `"Germany"` | Required. Only these 4 |
| `state` | string | `"TX"`, `"CA"` | **2-letter codes ONLY** |
| `city` | string | `"Austin"` | Narrows pool |
| `age_min` | integer | 25 | Recommended |
| `age_max` | integer | 55 | Recommended |
| `gender` | string | `"male"`, `"female"`, `"non_binary"` | Optional |
| `is_parent` | boolean | `true`, `false` | Optional |
| `education` | string | `"bachelors"`, `"masters"` | Optional |
| `industry` | array | `["Technology"]` | Optional |

**NOT supported:** `income`, `employment`, `ethnicity`, `political_affiliation`.

## Polling Strategy

| Study size | First poll delay | Subsequent interval |
|-----------|-----------------|---------------------|
| 10 personas | 45-50 seconds | 20 seconds |
| 12+ personas | 50-60 seconds | 20 seconds |

Poll ONE job_id as proxy — all jobs finish together.

## Response Structure

**Study creation:** `response["study"]["id"]` (nested under `study` key)

**Question responses:** `response_text` (may contain HTML: `<b>`, `<ul>`),
`agent_name`, `agent_age`, `agent_occupation`, `agent_summary`, `agent_city`.

**Share link:** Prefer `share_link` over `share_url`.

## Common Mistakes

- Publishing generic commentary with a study link (fails content quality test)
- Batching questions (produces shallow, disconnected answers)
- Using marketing language in `llmSummary` (must be encyclopaedic)
- Missing UTM parameters on share links
- Articles under 1,000 words (too thin for SEO/GEO)
- Not extracting content building blocks before writing
- Using `size` instead of `group_size`
- Using `response["id"]` instead of `response["study"]["id"]`
- Polling every 10-15s (use 45-50s first, then 20s)
- Skipping the `complete` step

## Limitations

Ditto personas have NOT used your specific product. For:
- Product-specific testimonials → use real customers
- Exact market sizing data → use industry reports
- Brand awareness metrics → use real surveys
- SEO keyword volume data → use SEO tools

**Recommended hybrid:** Ditto for research-backed content at scale
(the insights and quotes). Real data for metrics and exact numbers.

## Further Reading

- Full API guide: https://askditto.io/claude-code-guide
- Question design: @question-playbook.md
- SEO/GEO checklist: @advanced/seo-geo-optimisation.md
