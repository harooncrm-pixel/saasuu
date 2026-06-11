# Sprint 1: First 2 Weeks — Foundation Sprint

**Objective:** Establish legal entity, assemble founding team, validate core WASM strategy, and achieve first working PDF processing prototype.

**Target Outcomes by End of Week 2:**
- ✅ Company incorporated (Delaware C-corp)
- ✅ Founding team commitments (CEO, CTO, Designer, Growth)
- ✅ $500K-$1M pre-seed funding commitment in hand (or clear path)
- ✅ First working WASM PDF compress prototype (sub-100ms for 5MB file)
- ✅ Design system initialized (Radix UI + Tailwind boilerplate)
- ✅ Monorepo structure live (Turborepo + Cargo workspace)

---

## Week 1: Founding & Infrastructure

### Day 1 (Monday) — Incorporation & Legal Groundwork

**Morning (3 hours):**
- [ ] File Delaware C-corp incorporation (use Stripe Atlas or LawFirm)
- [ ] Assign cap table: Founder split (CEO 40%, CTO 30%, Designer 15%, Growth 15% — adjust for actual commits)
- [ ] Create ESOP plan (10-12% pool for future hires)
- [ ] Open business bank account (Mercury or Brex)
- **Checkpoint:** Proof of incorporation filed + bank account created

**Afternoon (3 hours):**
- [ ] Create GitHub org: `github.com/omnidoc-ai` (or your chosen name)
- [ ] Set up GitHub Projects board (Kanban: Backlog, Sprint, In Progress, Review, Done)
- [ ] Initialize Slack workspace: #general, #engineering, #product, #fundraising, #shipping
- [ ] Create shared Google Drive folder: `/OmniDoc` with subfolders: /Fundraising, /Product, /Engineering, /Hiring
- **Checkpoint:** GitHub org + Slack set up, shared workspace created

---

### Day 2 (Tuesday) — Team Alignment & Strategy Lock-in

**Morning (4 hours):**
- [ ] All 4 founders meet synchronously (2 hours)
  - Confirm roles and decision-making authority
  - Lock in company name (can change later, but commit now)
  - Review 30-step plan: any fundamental disagreements?
  - Agree on OKRs for Month 6: **50K MAU, <1s WASM processing, $0 spend on server costs**
  - **Decisions to make:** Primary market (B2B/B2C/both)? Geographic focus? First feature parity target (vs Adobe/SmallPDF)?
- [ ] Update `/OmniDoc/Product/MISSION.md` with agreed positioning statement
- **Checkpoint:** Mission locked, OKRs documented, team alignment confirmed

**Afternoon (2 hours):**
- [ ] CTO: Create tech decision matrix (see Critical Decisions doc) — assign one person to deep-dive each by EOD Friday
- [ ] Designer: Set up Figma project. Create 3 quick wireframes:
  - Homepage (positioning + tool showcase)
  - Single tool page (compress tool as reference)
  - Dashboard (after login, document hub preview)
- **Checkpoint:** Tech decisions assigned, Figma wireframes created

---

### Day 3 (Wednesday) — Pre-seed Fundraising Kick-off

**Morning (3 hours):**
- [ ] Create deck outline (Pitch deck, not investor docs yet — 15 slides)
  - Problem/opportunity
  - Market size ($10B TAM)
  - Product vision (3-min product video reference)
  - Competitive positioning (why we win)
  - Revenue model + unit economics
  - Team + why we can execute
  - Funding ask ($500K-$1M)
  - Use of funds (breakdown: salaries, infra, tools, marketing budget)
- [ ] Growth/CEO: Create angel/accelerator target list
  - 50 angels from Y Combinator network (AlsoVC, Lerer, Bessemer scouts)
  - 3-5 accelerators: Y Combinator, Techstars, Plug & Play
  - 5-10 founder friends/advisors who've done this before
- **Checkpoint:** Deck outline + investor list created

**Afternoon (2 hours):**
- [ ] Growth: Book 3 coffee calls with founder-mentors (existing network)
  - Goal: 1-2 intro warm intros to pre-seed angels each
  - Collect feedback on positioning
- **Checkpoint:** 3 calls booked

---

### Day 4 (Thursday) — Core Tech Decisions & WASM Setup

**Morning (4 hours) — Engineering work begins:**
- [ ] CTO: Set up monorepo structure
  ```
  omnidoc/
  ├── crates/
  │   ├── pdf-core/           (Rust + WASM — compress, merge, split)
  │   ├── pdf-ocr/            (Tesseract WASM bindings)
  │   └── Cargo.toml
  ├── apps/
  │   ├── web/                (Next.js 15 frontend)
  │   ├── desktop/            (Tauri + shared web code)
  │   └── api/                (Rust/Axum backend — future)
  ├── packages/
  │   ├── ui/                 (Shadcn/Radix design system)
  │   └── shared/             (Types, utilities)
  ├── turbo.json
  ├── package.json
  └── Cargo.toml
  ```
- [ ] CTO: Initialize and validate Rust WASM pipeline
  ```bash
  cargo new crates/pdf-core
  wasm-pack build crates/pdf-core --target bundler
  ```
  - Target: Can you see "Hello WASM" in Next.js console? (POC)
- [ ] CTO: Benchmark PDF.js vs pspdfkit-wasm vs custom Rust build
  - Test on a 5MB, 50-page PDF
  - Time: compress, render, extract pages
  - Create `/docs/WASM_BENCHMARK_DAY_4.md` with results
- **Checkpoint:** Monorepo structure live + first WASM "Hello World" + benchmark doc started

**Afternoon (2 hours):**
- [ ] Frontend: Initialize Next.js 15 app in `/apps/web/`
  - Set up Tailwind + Radix UI starters
  - Create landing page skeleton (positioning statement visible)
  - Deploy to Vercel (free tier, auto-deploy from main)
- [ ] Frontend: Create `/docs/DESIGN_SYSTEM.md` — document color palette, typography, component library roadmap
- **Checkpoint:** Next.js app on Vercel + design system documented

---

### Day 5 (Friday) — End-of-Week Showcase & Decisions Lock

**Morning (3 hours):**
- [ ] CTO: **CRITICAL WASM DECISION** — Finalize PDF.js vs custom Rust approach (see Critical Decisions)
  - Default: PDF.js + Rust WASM layer for compress/encrypt/ocr
  - Fallback: Custom Rust PDF parser (riskier but potentially faster)
  - Document decision in `/docs/ADR_001_WASM_APPROACH.md`
- [ ] CTO: Achieve first milestone: "Compress 5MB PDF in <500ms locally" (in browser console)
  - If <100ms → celebrate ✅
  - If 100-500ms → acceptable, optimize in Sprint 2
  - If >500ms → escalate, may need to pivot PDF library
- [ ] **Tech sync (1 hour):** All 4 founders
  - WASM performance review — are we hitting targets?
  - Database choice confirmation (PostgreSQL? SQLite for MVP? DuckDB?)
  - Auth system: Clerk vs Auth0 vs custom (recommend Clerk)

**Afternoon (2 hours):**
- [ ] **Team Sync & Week 1 Retro (1 hour)**
  - What worked? What didn't?
  - Update GitHub Projects: move completed tasks to Done
  - Plan pivots for Week 2
- [ ] Designer: Present 3 homepage wireframes → collect feedback
  - Iterate based on feedback
- [ ] Growth: Report on fundraising progress
  - # of warm intros in pipeline?
  - Any meetings booked for Week 2?
- **Checkpoint:** Week 1 demo + retro complete

---

## Week 2: MVP Foundation & Team Expansion

### Day 6 (Monday) — Full WASM Compress Tool & First Paid User

**Morning (4 hours):**
- [ ] Frontend: Build full compress tool UI
  - File upload (drag & drop)
  - Compression level selector (Lossless / Balanced / Aggressive / Max)
  - Real-time size preview
  - Download button
- [ ] CTO: Integrate WASM compress pipeline
  - Hook WASM fn to React component
  - Measure end-to-end latency (upload → compress → ready to download)
  - Target: <1s for 5MB file

**Afternoon (3 hours):**
- [ ] **Soft launch on Product Hunt community/Slack groups**
  - "We built a browser-based PDF compressor that works 100% offline"
  - Share link to `/web` Vercel deployment
  - Collect 5-10 user feedback snippets
- [ ] Growth: Send compress tool to 20 early-access users (friends, Twitter followers, Reddit)
  - Offer: "Free forever, no ads. We're building the PDF platform of the future. Send feedback: feedback@omnidoc.ai"
- **Checkpoint:** Compress tool live + 20 users invited

---

### Day 7 (Tuesday) — Merge Tool & Payment Setup

**Morning (3 hours):**
- [ ] Frontend: Build merge tool UI (file upload, drag-to-reorder, download)
- [ ] CTO: Integrate WASM merge pipeline
- [ ] Growth: Register Stripe account
  - Set up payment processing (for future Pro tier)
  - Create subscription SKU: Pro $10/mo, Team $8/user/mo

**Afternoon (3 hours):**
- [ ] Marketing: Publish first blog post: "Why Browser-Based PDF Tools are the Future" (500 words)
  - Publish on Medium, Dev.to, LinkedIn
  - Seed on Reddit (r/PDF, r/tools, r/SaaS)
- [ ] Growth: Collect user feedback from Day 6 → create GitHub Issues for each feature request
- **Checkpoint:** Merge tool live + payment processor set up + first content published

---

### Day 8 (Wednesday) — Hire First Employees

**Morning (3 hours):**
- [ ] CEO + CRO: **Go/no-go decision on hiring CTO #2 (backend engineer)**
  - If yes → post CTO job description (see document below)
  - Target profile: mid-level Rust engineer with API/backend exp
  - Outreach strategy: LinkedIn, Rust forums, Hacker News
- [ ] CEO: **Fundraising sprint**
  - Goal: Convert 3 warm intros into meetings this week
  - Prepare elevator pitch: "We're building the AI-native PDF platform. Free local processing. Private by design. $10B TAM. We'll own 8-12% in 5 years."

**Afternoon (2 hours):**
- [ ] Designer: Create brand assets
  - Logo (simple, rememberable)
  - 3-color palette
  - Social media templates
- [ ] Growth: Post on Twitter/X announcing the project
  - Tag relevant audiences: #PDF #WebDev #Rust #ProductHunt
  - Aim for 100+ impressions
- **Checkpoint:** Hiring started + brand assets created + fundraising in motion

---

### Day 9 (Thursday) — Second Tool (Split) & Auth System

**Morning (4 hours):**
- [ ] Frontend: Build split tool (page range input, download)
- [ ] CTO: Integrate WASM split pipeline
- [ ] Growth: Implement Clerk auth on `/apps/web/`
  - User registration/login flow live
  - User can save/manage uploaded files in browser storage (IndexedDB)
  - Create `/docs/AUTH_FLOW.md` documenting Clerk integration

**Afternoon (3 hours):**
- [ ] Growth: Check user feedback dashboard (Google Form? Typeform?)
  - Any clear pain points emerging?
  - Which tool is most requested next?
- [ ] CTO: Begin design for API schema (REST + GraphQL) — not implement, just document `/docs/API_DESIGN.md`
  - Endpoints: compress, merge, split, convert (future)
  - Usage-based pricing: how to meter?
- **Checkpoint:** Split tool + auth system live

---

### Day 10 (Friday) — End-of-Sprint 1 Demo & Metrics Review

**Morning (2 hours):**
- [ ] **All hands meeting (1 hour)**
  - Demo: Compress, Merge, Split tools (all working locally)
  - Show metrics: # of unique users since launch, avg session duration, top 3 requested features
  - Celebrate wins!

**Afternoon (3 hours):**
- [ ] **Data collection & analysis**
  - Metrics dashboard (PostHog or Mixpanel): MAU, Daily Active Users, Feature usage
  - Target: 100-500 users in first week
- [ ] **Plan Sprint 2 (next 2 weeks): OCR, Convert, Edit, AI features**
- [ ] Growth: Aggregate feedback into prioritized product roadmap
  - "Most requested feature by Week 2" becomes Sprint 2 priority
- **Checkpoint:** Sprint 1 complete. Metrics locked. Sprint 2 roadmap defined.

---

## Key Metrics to Track (End of Sprint 1)

| Metric | Target | Success Threshold |
|---|---|---|
| Total users registered | 1,000 | >500 |
| Daily active users | 100 | >50 |
| Tools launched | 3 (compress, merge, split) | All 3 working |
| Avg WASM compress latency | <500ms | <2s acceptable |
| Fundraising status | 2-3 investor meetings | 1 warm intro |
| Team hires | 1 Rust backend engineer committed | In process |
| Git commits | 50+ | Active development |
| Blog impressions | 1k+ | >500 |

---

## Daily Standup Template (10 min, async in Slack)

```
[NAME] - [DATE]
Yesterday: [what you shipped]
Today: [what you're shipping]
Blockers: [if any]
Morale: 😊 / 😐 / 😞
```

Post in #shipping every EOD.

---

## Resources & Links to Create/Lock Down

- GitHub: https://github.com/omnidoc-ai
- Vercel deployment: https://omnidoc-web.vercel.app (or your domain)
- Slack: #shipping, #fundraising, #product
- Figma: Design project link
- PostHog: Analytics dashboard
- Stripe: Payment processing
- Clerk: User auth

---

## Red Flags (Escalate Immediately if Any Occur)

🚨 WASM compress latency >2s for 5MB file → May need to pivot PDF library or compress algo
🚨 Fewer than 50 users by Day 10 → Marketing messaging not resonating; rethink positioning
🚨 CTO hire stalled by Day 8 → Expand search (LinkedIn + Slack communities)
🚨 No warm investor intros by Day 7 → Pivot: reach out directly to angel networks (AngelList, etc.)

---

## End-of-Sprint Retrospective Template

**What went well?**
- [2-3 wins]

**What didn't?**
- [2-3 challenges]

**What will we do differently in Sprint 2?**
- [3 changes]
