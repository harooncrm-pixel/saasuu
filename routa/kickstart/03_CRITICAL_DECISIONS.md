# 5 Critical Technical Decisions — Must Make TODAY

These decisions block every other choice. If you get these wrong, you waste months.

---

## Decision 1: PDF Rendering Engine

**Options:**

| Option | Build Time | Cost | Performance | Moat | Risk |
|--------|-----------|------|-------------|------|------|
| **A. Custom WASM (fork PDFium → compile to WASM)** | 3-4 months | ~$200K (1 Rust eng, 4mo) | ~4x faster than PDF.js | ⭐⭐⭐⭐⭐ Strong moat | High — PDF spec complexity |
| **B. Apryse/PDFTron SDK** | 2 weeks | ~$50K/yr license | Excellent | ⭐ No moat | Low |
| **C. PDF.js (open-source, Mozilla)** | 1 week | Free | Adequate for MVP, poor on large files | ⭐ No moat | Low |
| **D. PSPDFKit WASM** | 2 weeks | ~$40K/yr | Best-in-class | ⭐ No moat | Low — vendor dependency |

**Recommendation: A (custom WASM) for the MVP, but with B as backup.**

Build a thin WASM wrapper around PDFium (Apache 2.0). Start with rendering only (PDFium is battle-tested for rendering). This gives you:
- Full control over performance (tile-based, GPU compositing)
- No per-seat licensing costs (critical for free tier economics)
- Long-term moat (you own the engine)

**Fallback:** If after 6 weeks the WASM engine is not rendering at acceptable quality, license Apryse. Don't waste 4 months on a blocked engine.

**Decision needed:** Do we commit 1 Rust engineer to WASM for 6 weeks, or license Apryse from Day 1?

---

## Decision 2: AI Model Strategy

**Options:**

| Approach | Latency | Cost/Query | Privacy | Quality |
|----------|---------|-----------|---------|---------|
| **A. GPT-4o only** | Fast | ~$0.03 | Cloud only (users' data leaves) | Best |
| **B. Llama 4 (local) + GPT-4o fallback** | Local: slow/GPU | ~$0.001 (local) or ~$0.03 (cloud) | Local option for private docs | Good |
| **C. Claude 4 only** | Fast | ~$0.04 | Cloud only | Best on documents |
| **D. Abstracted router: all providers + local** | Flexible | Optimized per task | Configurable | Flexible |

**Recommendation: D — Abstracted model router from Day 1.**

Build a thin routing layer (`packages/ai-client`) that:
- Routes "summarize" to Llama 4 (cheap, fast)
- Routes "contract analysis" to Claude 4 (best at documents)
- Routes "OCR" to a fine-tuned vision model
- Routes privacy-sensitive docs to local inference only

This is ~200 lines of TypeScript. Don't hardcode a single provider.

**Decision needed:** Which base models for each tier? (Suggested: GPT-4o-mini for chat, Claude 4 for contracts, Llama 4 for on-prem.)

---

## Decision 3: Real-Time Collaboration Protocol

**Options:**

| Approach | Offline Support | Complexity | Ecosystem | Conflict Resolution |
|----------|----------------|------------|-----------|-------------------|
| **A. CRDT (Yjs)** | ✅ Native | Medium | Mature (Yjs + y-websocket) | Automatic via CRDT |
| **B. OT (Operational Transform)** | ❌ Requires server | High | Google Docs-style | Requires central server |
| **C. Locking (pessimistic)** | ❌ | Low | Simple | User-level locks; no true collab |

**Recommendation: A — Yjs (CRDT).**

Otto (OT) is harder to implement correctly, and CRDTs handle offline edits natively — a key product differentiator. Yjs is battle-tested (used by Linear, Roam, Notion-ish setups).

**Build for Day 1 even if single-user MVP:** The architecture decision (CRDT-based document model) affects how you store, diff, and sync document state. If you pick locking now and switch to CRDT later, you rewrite the data layer.

**Decision needed:** Confirm Yjs as the collaboration primitive, even for single-user MVP. This means all document state must be representable as a CRDT-compatible data structure.

---

## Decision 4: Backend Language — Rust vs. Go vs. Node.js

**Options:**

| Language | Performance | Ecosystem | Hiring | Development Speed |
|----------|------------|-----------|--------|-------------------|
| **Rust** | Fastest, memory-safe | Young but growing | Hard (expensive, scarce) | Slow |
| **Go** | Very fast | Mature | Medium | Fast |
| **Node.js** | Adequate | Largest | Easy | Fastest |

**Recommendation: Rust for document/memory-safe services, Node.js for API routes that don't touch PDF bytes.**

Split strategy:
- **Document Service → Rust** (memory-safe PDF processing, high-throughput, no GC pauses)
- **AI Service → Rust** (ONNX Runtime, llama.cpp bindings, memory efficiency)
- **API Gateway / Auth → Go or Node.js** (I/O-bound, thin logic, fast hiring)
- **Web App → Next.js** (obvious choice)

**Don't go all-Rust or all-Node.** The split buys you performance where it matters and hiring speed where it doesn't.

**Decision needed:** Confirm Rust for core services, Node/Go for edge. If you cannot hire Rust engineers in your market, reconsider.

---

## Decision 5: Open-Source Strategy

**Options:**

| Model | Community Adoption | Revenue Protection | Complexity |
|-------|-------------------|-------------------|------------|
| **A. AGPL + commercial license** | Strong (AGPL forces contribution) | Requires commercial license for proprietary use | Medium |
| **B. BSL 1.1 (MongoDB-style)** | Moderate | Source-available, converts to Apache after 3 years | Low (standard) |
| **C. MIT / Apache 2.0** | Strongest | No protection — competitors can resell | None |
| **D. Source-available only (no license)** | Weakest | Full protection | High (confuses community) |

**Recommendation: B — BSL 1.1.**

- Source is available (builds trust)
- Converts to Apache 2.0 after 3 years (community-friendly)
- Commercial use requires license (protects revenue)
- Used by MongoDB, CockroachDB, Sentry — well-understood by enterprises
- Makes the "open-source core" marketing claim legitimate

**What to open-source:**
- `packages/pdf-engine` (WASM renderer) — open-source core
- `packages/shared-types` — open-source
- `apps/web` — only the SDK/embed parts (not the full product UI)

**What to keep proprietary:**
- `services/` — backend microservices
- AI prompt chains, model configurations
- eSignature PKI infrastructure

**Decision needed:** BSL 1.1? Or a different model? Any legal constraints?
