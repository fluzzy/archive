---
name: Claude Ads — Paid Advertising Audit Plugin
description: Claude Code plugin that runs 250+ automated audit checks across 7 paid ad platforms with parallel subagents and weighted health scoring
type: archive
---

# Claude Ads: Paid Advertising Audit Skill for Claude Code

> **Source**: [Agrici Daniel](https://github.com/AgriciDaniel/claude-ads)
> **Author**: Agrici Daniel
> **Fetched**: 2026-05-04
> **Archived**: 2026-05-04

Open-source Claude Code plugin that runs 250+ automated audit checks across Google, Meta, YouTube, LinkedIn, TikTok, Microsoft, and Apple Ads. Replaces traditional 2–4 week, $5K–$10K agency audits with a 3–5 minute local analysis that produces a 0–100 health score, prioritized action plan, and PDF-ready client deliverables.

---

## Installation

**Plugin install (recommended):**

```shell
/plugin marketplace add AgriciDaniel/claude-ads
/plugin install claude-ads@agricidaniel-claude-ads
```

**One-command install (Unix/macOS/Linux):**

```bash
curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-ads/main/install.sh | bash
```

**One-command install (Windows PowerShell):**

```powershell
irm https://raw.githubusercontent.com/AgriciDaniel/claude-ads/main/install.ps1 | iex
```

Manual install via `git clone` + `./install.sh` is also supported.

---

## Commands

| Command | Description |
| --- | --- |
| `/ads audit` | Full multi-platform audit with parallel subagent delegation |
| `/ads google` | Google Ads deep analysis (Search, PMax, Display, YouTube, Demand Gen) |
| `/ads meta` | Meta Ads deep analysis (FB, IG, Advantage+ Shopping) |
| `/ads youtube` | YouTube Ads (Skippable, Shorts, Demand Gen) |
| `/ads linkedin` | LinkedIn Ads (B2B, Lead Gen, TLA) |
| `/ads tiktok` | TikTok Ads (Creative, Shop, Smart+) |
| `/ads microsoft` | Microsoft/Bing Ads (Copilot, Import validation) |
| `/ads apple` | Apple Ads (campaign structure, CPPs, Maximize Conversions, TAP) |
| `/ads creative` | Cross-platform creative quality audit and fatigue detection |
| `/ads landing` | Landing page quality assessment for ad campaigns |
| `/ads budget` | Budget allocation and bidding strategy review |
| `/ads plan <type>` | Strategic ad plan with industry templates |
| `/ads competitor` | Competitor ad intelligence across platforms |
| `/ads math` | PPC financial calculator (CPA, ROAS, break-even, LTV:CAC) |
| `/ads test` | A/B test design (hypothesis framework, significance, sample size) |
| `/ads report` | Generate PDF audit report for client deliverables |

---

## `/ads audit` — Parallel Multi-Platform Audit

Spawns 6 parallel subagents that analyze ad accounts simultaneously:

- **audit-google** — 80 checks across Search, PMax, AI Max, Demand Gen, CTV, YouTube
- **audit-meta** — 50 checks across Pixel/CAPI, Andromeda creative diversity, structure, audience
- **audit-creative** — 21+ cross-platform creative quality checks (Andromeda + Symphony aware)
- **audit-tracking** — 8+ conversion-tracking and privacy-infrastructure checks (Consent Mode V2, CAPI, Events API, AdAttributionKit)
- **audit-budget** — 24 budget and bidding-strategy checks
- **audit-compliance** — 18+ compliance checks (ECPC deprecated, VAC deprecated, EU messaging, Apple rebrand)

Output: a unified **Ads Health Score (0–100)** with prioritized action plan.

---

## Audit Coverage (250+ Checks)

| Platform | Checks | Key Areas |
| --- | --- | --- |
| Google Ads | 80 | Search, PMax, AI Max, Demand Gen, CTV, YouTube |
| Meta Ads | 50 | Pixel/CAPI, Andromeda creative diversity, structure, audience |
| Apple Ads | 35+ | Campaign structure, CPPs, Maximize Conversions, AdAttributionKit |
| TikTok Ads | 28 | Creative-first, Smart+, GMV Max, Search Ads, Events API |
| LinkedIn Ads | 27 | B2B targeting, TLA, Lead Gen, CRM integration |
| Microsoft Ads | 24 | Google import safety, Copilot, CTV, LinkedIn targeting, video |
| Cross-platform | 3 | Privacy infrastructure, creative diversity, refresh cadence |

### Health Score Grading

| Grade | Score | Action Required |
| --- | --- | --- |
| A | 90–100 | Minor optimizations only |
| B | 75–89 | Some improvement opportunities |
| C | 60–74 | Notable issues need attention |
| D | 40–59 | Significant problems present |
| F | <40 | Urgent intervention required |

Severity multipliers (Critical 5.0×, High 3.0×, etc.) weight checks before scoring.

---

## `/ads plan <business-type>` — Strategic Templates

Industry-specific templates covering platform mix, campaign architecture, creative strategy, targeting, budget guidelines, and KPI targets.

| Type | Focus |
| --- | --- |
| `saas` | Trial/demo focus, Google + LinkedIn primary |
| `ecommerce` | Shopping/PMax, ROAS-focused, seasonal |
| `local-service` | Google Search + LSA, call tracking, geo radius |
| `b2b-enterprise` | LinkedIn ABM, long sales cycle, pipeline metrics |
| `info-products` | Meta + YouTube, webinar/VSL funnels |
| `mobile-app` | Meta + Google UAC, MMP required, LTV:CPI |
| `real-estate` | Special Ad Category (housing), buyer/seller campaigns |
| `healthcare` | HIPAA compliance, LegitScript, restricted targeting |
| `finance` | Special Ad Category (credit), required disclosures |
| `agency` | Multi-client management, reporting framework |
| `generic` | Universal template with platform selection questionnaire |

---

## Quality Gates (Enforced Every Audit)

- Never recommend Broad Match without Smart Bidding (Google)
- 3× Kill Rule: flag CPA >3× target for immediate pause
- Budget sufficiency: Meta ≥5× CPA/ad set, TikTok ≥50× CPA/ad group
- Learning phase protection: no edits during active learning
- Special Ad Category compliance auto-check (housing/credit/finance)
- Privacy infrastructure gate: verify Consent Mode V2, CAPI, Events API, AdAttributionKit before optimization recs
- Andromeda creative diversity: flag Meta accounts with <10 genuinely distinct creatives

---

## Architecture

```
~/.claude/skills/ads/              # Main orchestrator
~/.claude/skills/ads/references/   # 25 RAG reference files
~/.claude/skills/ads-*/            # 19 sub-skills (17 original + ads-math + ads-test)
~/.claude/skills/ads-plan/assets/  # 12 industry templates
~/.claude/agents/                  # 10 agents (6 audit + 4 creative)
```

**Three-layer design:**

1. **Directive** — orchestrator + quality gates
2. **Orchestration** — routes between 19 sub-skills
3. **Execution** — 10 agents, 25 reference files, 12 industry templates

References load on-demand (RAG pattern) — only what's needed per analysis.

---

## Data Handling & Privacy

- All analysis runs locally via Claude Code; no ad data is sent to external servers
- Tool works with data the user provides (exports, screenshots, pasted metrics) — does not auto-connect to ad platforms
- Optional MCP integrations for live API data flow directly between local machine and platform APIs:
  - Google Ads: [mcp-google-ads](https://github.com/cohnen/mcp-google-ads) (29 GAQL tools)
  - Meta Ads: Adspirer MCP or bundled `scripts/fetch_meta_ads.py`
  - LinkedIn Ads: GrowthSpree MCP or Adzviser MCP

---

## Output Format

Results are **prioritized issue lists sorted by estimated revenue impact** — not slide decks. Each issue includes:

- Specific problem and severity
- Step-by-step fix instructions
- Estimated monthly savings
- "Quick Win" flag for issues fixable in <15 minutes

Typical findings: 15–30% wasted ad spend from missing conversion tracking, budget misallocation, or audience overlap. On a $50K/month budget, that translates to $7,500–$15,000 in monthly savings.

---

## Requirements

- Claude Code CLI
- Python 3.10+ with Playwright (optional, for live landing page analysis)
- reportlab (optional, for PDF reports via `/ads report`)

---

## License

MIT.

---

## References

- [GitHub repository](https://github.com/AgriciDaniel/claude-ads)
- [Author blog post: Claude Code as an ad agency replacement](https://agricidaniel.com/blog/claude-code-ad-agency)
- [Author profile](https://agricidaniel.com/about)
- [GeekNews summary (Korean)](https://news.hada.io/topic?id=29022) — xguru, 2026-04-30
