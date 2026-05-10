# AiToEarn Case Study: AI Content Monetization Platform (v2.1, May 2026)

**Repo:** https://github.com/yikart/AiToEarn | **Stars:** ~10K | **License:** MIT | **Tech:** TypeScript + Next.js + NestJS + MongoDB + Redis | **Topics:** douyin, xiaohongshu, kuaishou, electron

## Summary
AiToEarn is an AI-powered content marketing agent platform for OPC (one-person companies). Four Agent capabilities: Create (AI content gen), Publish (multi-platform distribution to 12+ platforms), Engage (automated interaction), Monetize (content marketplace).

## Business Model

### Platform Revenue: Credits System (Pay As You Go)
| Tier | Price | Credits |
|------|-------|---------|
| Basic | ¥70 | 1,000 |
| Recommended | ¥350 | 5,000 |
| Premium | ¥700 | 10,000 |

100 Credits = $1 USD. Covers: LLM tokens + third-party APIs (video/image gen). Platform passes through model costs, profit from volume discounts.

### LLM Model Pricing (per million tokens, paid in Credits)
| Model | Input | Output |
|-------|-------|--------|
| GPT-5 | $1.25 | $10 |
| GPT-5.1 | $1.25 | $10 |
| Claude Sonnet 4.5 | $3 | $15 |
| Claude Opus 4.5 | $5 | $25 |
| Gemini 3.0 Pro Preview | $2 | $12 |

### Video Generation (what 1,000 Credits ≈ can generate)
| Model | Duration |
|-------|----------|
| Seedance 2.0 | 66s |
| HappyHorse 1.0 | 76s |
| Grok Video 10s | 1,111s |

### Marketplace Commission
| Task Type | Payout | Example |
|-----------|--------|---------|
| CPE | Per engagement | $0.80/interaction, capped at $20,000/post |
| CPM | Per 1K views | $0.10–$1.00/1K views |
| Fixed Price | Per post | $0.50–$5.00/post |
| Interaction | Per task | $0.50/task |

### Real Marketplace Snapshot (May 2026)
- **Most tasks sell out instantly.** Only 1 pinned task was available.
- Available: **Jimeng promotion task** (CPE, $0.80/interaction, $20K cap/post)
- Fixed Price range: $0.50 (10-slot) to $5.00 (1-slot)
- CPM range: $0.10/1K views (Twitter, Insta) to $1.00/1K views (YouTube)
- **Key insight**: AiToEarn posts most tasks itself ("Promote AITOEARN" series) — bootstrapping marketplace
- Filters: Platform, Tags, Country/Region, Date Range, keyword search
- Tabs: Creator (accept) / Advertiser (publish)

### Real Task Acceptance: AiToEarn Self-Promotion on Xiaohongshu (May 2026)
A user accepted a task promoting AiToEarn itself on 小红书 (Xiaohongshu):

| Field | Value |
|-------|-------|
| Task Type | CPE (每千次互动) |
| Total Reward | **¥50.00** |
| Platform Fee | 3% |
| Withdrawal Fee | 7% |
| Status | 进行中 (In Progress - accepted, content posted, awaiting review) |
| Title | "AitoEarn:一人公司AI变现利器" |
| Hashtags | #一人公司 #AI自动化 #商业变现 #AitoEarn #生产力工具 |
| Platform | 小红书 (Xiaohongshu) |

**Content description provided by task:** Promotes AiToEarn's four core features (Monetize, Publish, Engage, Create), positions it as an OPC AI content marketing agent, and frames it as a productivity revolution for solopreneurs.

**Key fee insight:** Total platform burden = 3% + 7% = 10%. On a ¥50 task, ~¥5 goes to fees. Net ~¥45.

## Three Implementation Paths

### Path 1: Direct User (Recommended First)
1. Register at aitoearn.cn, get API Key (¥70 min top-up)
2. Use via OpenClaw plugin (user has OPC system deployed)
3. Accept small Fixed Price tasks to learn flow
4. Costs: ~¥70. Time: ~1h/day.

### Path 2: Docker Self-Deploy
- `docker compose up -d` on user's JD Cloud server
- Needs Relay config to borrow official OAuth credentials
- Can configure own AI models (cheaper Chinese models via new-api/one-api)

### Path 3: MCP Integration with Hermes Agent
- MCP endpoint: `https://aitoearn.ai/api/unified/mcp` with x-api-key header
- User's OPC system becomes: Collect → Analyze → Create → Publish

## Key Risks
- Platform task supply is limited; most tasks SOLD OUT instantly
- Account bans on social platforms (automated publishing)
- Low per-task payout needs scale for meaningful income
- AI API credit costs can eat into margins at low volumes

## URLs
- CN: https://aitoearn.cn
- Intl: https://aitoearn.ai
- GitHub: https://github.com/yikart/AiToEarn

## Network Note
GitHub raw content (raw.githubusercontent.com) unreliable from JD Cloud server. Use GitHub API (api.github.com) instead — more reliable for README/content fetching.
