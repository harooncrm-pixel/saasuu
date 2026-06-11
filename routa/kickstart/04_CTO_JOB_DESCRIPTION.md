# Job Description: Chief Technology Officer (CTO) — OmniDoc

## Role Summary

**Title:** Chief Technology Officer (CTO) / Co-Founder (CTO)
**Stage:** Pre-seed / Seed (we have strategic plan + funding lead)
**Location:** Remote-first (with quarterly offsites)
**Comp:** $140K–$180K base + 5–10% equity (co-founder/early-employee tier)
**Reports to:** CEO
**Direct reports initially:** 0 (will build team from scratch)
**Start:** Immediate (within 2 weeks)

---

## About OmniDoc

OmniDoc is building the AI-native document platform that replaces Adobe Acrobat, DocuSign, and your entire document workflow stack — at half the cost. We're competing in a $10B+ TAM with a thesis that PDF editing, e-signature, AI document processing, and real-time collaboration belong in ONE platform, not five.

We have:
- A fully researched strategic plan (market analysis, architecture, GTM, budget)
- Seed funding lined up (need CTO to close)
- Strategic partnerships in discussion with enterprise prospects

---

## What You'll Do (First 90 Days)

### Days 1–30: Foundation

- **Architecture lock-in:** Finalize the 5 critical technical decisions (WASM engine approach, AI model strategy, CRDT vs. OT, language split, open-source licensing)
- **Team assembly:** Hire first 4–6 engineers (Rust WASM engineer, backend/API lead, frontend lead, AI/ML engineer, DevOps)
- **Dev environment:** Set up repo, CI/CD, Docker Compose dev environment, cloud sandbox (AWS/GCP)
- **Sprint 1 delivery:** Ship MVP — working PDF viewer in browser with WASM engine + basic AI chat (the Sprint 1 plan exists — execute it)

### Days 31–60: MVP Completion

- Ship text editing + annotation capabilities
- Ship auth, document management, file upload/download
- Ship AI assistant with streaming Q&A (summarize, explain, translate)
- Establish engineering standards: code review, testing, performance budgets
- Set up observability (OpenTelemetry, Grafana, Sentry)

### Days 61–90: Market-Ready

- E2E flow: Upload → Edit → AI Chat → Save → Download is polished
- Free tier deployed and open for sign-ups
- Performance profiling: P95 render < 1s, WASM bundle < 5MB
- Sprint 2 plan loaded: eSignature MVP, team workspaces, OCR
- First enterprise beta customer onboarded

---

## Must-Have Skills (Non-Negotiable)

### Technical

| Skill | Why |
|-------|-----|
| **Systems programming (Rust or C++)** | You will personally architect and review the WASM PDF engine. Must understand memory management, WASM compilation, and rendering pipelines. |
| **WebAssembly production experience** | Not "I've heard of it." Have you shipped WASM to production? Debugged WASM memory leaks? Optimized bundle size? |
| **PDF specification knowledge** | You understand PDF 2.0 structure (cross-reference tables, content streams, font encoding, form XObjects). You know why the PDF spec is hard. |
| **AI/ML inference architecture** | You can design a tiered model router: local (llama.cpp) vs. cloud (OpenAI/Anthropic). You understand RAG, prompt caching, KV cache optimization. |
| **Cloud infrastructure (AWS/GCP)** | Kubernetes, Terraform, PostgreSQL, Redis, S3-compatible storage. You've run production services. |
| **System design at scale** | You've designed systems handling 10M+ requests/day. You understand CRDTs, distributed storage, WebSocket scaling, SSO/SAML. |

### Leadership

| Skill | Why |
|-------|-----|
| **Hired & managed 10+ person engineering teams** | We need to go from 0 → 15 engineers in 6 months. You've done this before. |
| **Built a B2B SaaS product from 0 → 1** | You know the difference between "cool tech" and "shippable product." You prioritize ruthlessly. |
| **Technical recruiting** | You can write a job description, assess a candidate's Rust/PDF/WASM skills accurately, and close hires without a recruiter on Day 1. |
| **Enterprise customer conversations** | You can join a sales call with a Fortune 500 CISO and explain our security architecture credibly. |
| **Open-source community building** | You understand how to build a community around an open-source core while protecting the commercial product. |

---

## Strongly Preferred (Not Required)

- Built a PDF tool before (worked at Foxit, Nitro, PDFelement, Apryse, PSPDFKit, Adobe)
- Experience with eSignature PKI (digital signatures, certificate chains, audit trails)
- Contributed to PDFium, Poppler, or similar open-source PDF library
- Published a paper or talk on CRDTs or collaborative editing
- Exited a startup as CTO/VP Engineering (acquisition or IPO)

---

## What We're NOT Looking For

- "AI wrapper" mindset — we are building deep tech (WASM engine, custom OCR, CRDT collab), not a thin GPT wrapper
- "Let's use Python for everything" — our core services need Rust/C++ performance
- "We'll figure out architecture later" — architectural decisions made in the first month propagate for years
- "I don't do code reviews" — you will review every PR until we hire senior engineers
- "Enterprise is not my focus" — 80% of our revenue comes from enterprise; you need to care about SSO, audit logs, compliance

---

## Compensation & Package

| Component | Details |
|-----------|---------|
| **Base salary** | $140K–$180K (pre-seed/seed range for founding CTO) |
| **Equity** | 5–10% (4-year vest, 1-year cliff) — co-founder range |
| **Benefits** | Health/dental/vision (100% covered), unlimited PTO, equipment budget ($5K), conference budget |
| **Deferred comp** | Option to defer salary for additional equity |
| **Location** | Remote-first (must overlap 4h+ with US Eastern / European time zones) |

**Why this comp range:** At pre-seed, you can't offer FAANG salaries. You offer:
1. Significant equity upside (5–10% of a $10B+ TAM opportunity)
2. Technical autonomy (you design the architecture, choose the stack, build the team)
3. Founding-team role (co-founder title, board observer seat, equal strategic voice)

---

## How to Evaluate Candidates

Technical screen (3 steps):

1. **WASM deep-dive (1h, pair programming):** Ask them to compile a simple C function to WASM, pass a buffer from JS, and render something on a canvas. If they can't do this, they can't lead our core engine team.

2. **PDF spec walk-through (1h, whiteboarding):** Hand them a malformed PDF. Ask: "What's wrong with this XREF table? How would you fix it in the renderer?" Look for deep understanding of PDF internals.

3. **System design: Document sync (1h, whiteboarding):** "Design real-time collaborative editing for a 100MB PDF with 10 concurrent editors." They should discuss CRDTs vs. OT, conflict resolution, delta encoding, WebSocket scaling, and offline support.

**Red flags:**
- "PDF is a solved problem" (it's not — the spec is 1,000+ pages)
- "Let's use a library for everything" (we need deep tech, not integrators)
- "CRDTs are too complex, let's use locking" (shows they don't understand the product vision)
- No experience shipping WASM to production

---

## How to Apply

Send:
1. GitHub profile / open-source work
2. One-paragraph answer: "What's the hardest technical problem you've solved related to documents, rendering, or real-time collaboration?"
3. Why OmniDoc specifically (not just "I want to be a CTO")

---

*This role is the single most important hire we will make. Get it right.*
