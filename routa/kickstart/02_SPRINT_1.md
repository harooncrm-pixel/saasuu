# Sprint 1: Foundation — Days 1-14

> Goal: Working PDF viewer in browser + local dev environment + team operational.
> Outcome: A PR that compiles and renders a PDF page in a `<canvas>` via WASM.

## Week 1: Scaffold & Core Engine

### Day 1 — Repository & Dev Environment

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Create mono-repo from `01_PROJECT_STRUCTURE.md` | Lead | 2h | Repo with npm workspaces + Cargo workspace |
| `setup-dev.sh` — Docker Compose for PG, MinIO, Redis | DevOps | 3h | `docker compose up -d` works |
| Root `README.md`, `CONTRIBUTING.md`, `LICENSE` | PM | 2h | Community-facing repo |
| CI pipeline (`.github/workflows/ci.yml`) | DevOps | 2h | Lint + test + build on PR |
| **Checkpoint @ 5pm:** `git commit -m "chore: initial scaffold"` | All | — | Repo is green on `main` |

### Day 2 — WASM PDF Engine: First Compilation

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Fork PDFium / set up `pdf-engine` Cargo.toml | Rust Eng | 4h | `cargo build` succeeds |
| WASM target: `wasm-pack build --target web` | Rust Eng | 3h | `.wasm` binary produced |
| Basic `lib.rs`: `render_page(pdf_bytes, page_num) → ImageData` | Rust Eng | 3h | Function compiles to WASM |
| **Checkpoint @ 5pm:** `wasm-pack build` produces deployable `.wasm` | Rust Eng | — | Size < 5MB (compressed) |

### Day 3 — JS Wrapper & Web Integration

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| `pdf-engine-js` — init WASM worker, expose `renderPage()` | Frontend | 4h | `npm run build` in package |
| Next.js app scaffold + `/editor/[id]` route | Frontend | 2h | Route renders blank page |
| Drag-and-drop file upload on landing page | Frontend | 2h | File picker → passes bytes to WASM |
| **Checkpoint @ 5pm:** Drop a PDF → canvas shows first page | Frontend | — | Working render pipeline |

### Day 4 — Page Navigation & Zoom

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Multi-page rendering (scroll + page thumbnails) | Frontend | 3h | All pages rendered |
| Zoom (fit-width, fit-page, percentage) | Frontend | 2h | Zoom slider works |
| Tile-based rendering (only render visible tiles) | Rust Eng | 3h | Lazy rendering for 100+ page docs |
| **Checkpoint @ 5pm:** Navigate 50-page PDF with smooth scroll | Frontend | — | No frame drops |

### Day 5 — Dev Backend: Document Upload & Storage

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| `document-service`: POST `/documents/upload` | Backend | 4h | Upload → store in MinIO → return ID |
| `document-service`: GET `/documents/:id/page/:num` | Backend | 2h | Proxy page bytes to frontend |
| PostgreSQL schema: `documents`, `users`, `sessions` | Backend | 2h | Migrations in `db/migrations` |
| **Checkpoint @ 5pm:** Upload a PDF via curl, fetch it back | Backend | — | Full upload → store → serve flow |

### Weekend Checkpoint (End of Week 1)

| Metric | Target | How to Verify |
|--------|--------|---------------|
| PDF renders in browser | ✅ | `npm run dev`, drag PDF, see page 1 |
| WASM engine builds | ✅ | `wasm-pack build` succeeds |
| Document upload works | ✅ | `curl -F "file=@test.pdf" localhost:3001/documents/upload` |
| CI green | ✅ | GitHub Actions passes |

## Week 2: Core Editing & AI Chat

### Day 6 — Text Layer Extraction & Selection

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Extract text layer from PDF page (via PDFium) | Rust Eng | 4h | `get_text_layer(page_num) → BBox[]` |
| Render text selection highlights on canvas | Frontend | 3h | Click-drag selects text |
| Copy selected text to clipboard | Frontend | 1h | Cmd+C copies text |
| **Checkpoint @ 5pm:** Select text on any PDF page, copy it | Rust + FE | — | Text is extractable |

### Day 7 — Basic Text Editing (Insert/Delete)

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Overlay content-editable div on selected text | Frontend | 3h | Click text → edit in place |
| Apply edit back to PDF (insert/delete glyphs) | Rust Eng | 4h | WASM `apply_text_edit()` creates new content stream |
| PDF save (download modified bytes) | Frontend | 1h | "Save" → download edited PDF |
| **Checkpoint @ 5pm:** Edit text in PDF, download, re-open | Rust + FE | — | Text change persists |

### Day 8 — AI Chat Interface (Frontend)

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| AI Chat panel UI (slide-out sidebar) | Frontend | 3h | Resizable chat panel with message list |
| Chat input + streaming response display | Frontend | 2h | SSE stream renders markdown |
| Page context injection (send current page text to LLM) | Frontend | 2h | "What does this page say?" works |
| **Checkpoint @ 5pm:** Ask "summarize this page" → get answer | Frontend | — | Chat works with page context |

### Day 9 — AI Service: LLM Integration

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| `ai-service` scaffold + OpenAI-compatible API | Backend | 3h | POST `/ai/chat` → upstream LLM |
| Tiered model routing (small model for summarize, large for deep QA) | Backend | 2h | Request routed by `intent` field |
| Streaming response (SSE) | Backend | 2h | `text/event-stream` output |
| Document context injection (extract page text → build prompt) | Backend | 2h | LLM gets page text + user question |
| **Checkpoint @ 5pm:** Chat endpoint responds to questions | Backend | — | `curl` → streamed answer |

### Day 10 — Auth Service

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Deploy Ory Kratos with PostgreSQL backend | Backend | 3h | Registration + login flow |
| Sign-up / sign-in pages (UI) | Frontend | 2h | Email + Google OAuth |
| JWT session management + middleware | Backend | 2h | Protected routes |
| **Checkpoint @ 5pm:** User can sign up, sign in, see their documents | Backend + FE | — | Auth flow end-to-end |

### Day 11 — Document List & Dashboard

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Document list page (grid view with thumbnails) | Frontend | 3h | Shows user's documents |
| Thumbnail generation (render first page at small size) | Rust Eng | 2h | `render_thumbnail()` WASM export |
| Delete, rename, duplicate documents | Frontend + Backend | 2h | CRUD complete |
| **Checkpoint @ 5pm:** Dashboard shows documents, CRUD works | FE + Backend | — | Full document lifecycle |

### Day 12 — Integration Polish

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| End-to-end: Upload → View → Edit → AI chat → Save → Download | All | 4h | Full workflow passes |
| Error states (upload failure, render failure, network error) | Frontend | 2h | Toast/snackbar for all errors |
| Loading states (spinner, skeleton) | Frontend | 1h | UX polish |
| Performance profile (P95 render time, WASM size) | Rust Eng | 2h | Benchmark numbers recorded |
| **Checkpoint @ 5pm:** Full demo walk-through | All | — | Product owner signs off |

### Day 13 — Buffer / Overflow / Bug Fixes

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Fix top-10 bugs from week's issue tracker | All | 5h | Bug count < 5 |
| Add missing unit tests | All | 2h | Coverage > 60% |
| Write ADR for any decisions made this sprint | Lead | 1h | `docs/adr/` populated |
| **Checkpoint @ 5pm:** All PRs merged, `main` green | All | — | Sprint complete |

### Day 14 — Sprint Review & Retro

| Task | Owner | Duration | Deliverable |
|------|-------|----------|-------------|
| Demo to stakeholders | PM | 1h | Recording + notes |
| Retrospective: what worked, what didn't | All | 1h | Action items |
| Sprint 2 planning | All | 2h | Sprint 2 backlog scoped |
| Update roadmap with refined estimates | PM | 1h | Roadmap baselined |

## Sprint 1 Definition of Done

- [ ] WASM PDF engine compiles and renders PDFs in browser
- [ ] Multi-page navigation with tile-based rendering works
- [ ] Text selection + copy works
- [ ] Basic text editing works (insert/delete)
- [ ] AI Chat sidebar works with page context injection
- [ ] Auth (signup/login) works
- [ ] Document upload/storage/CRUD works
- [ ] CI pipeline is green
- [ ] E2E test covers: upload → view → edit → AI chat → download
- [ ] Performance: P95 render < 1s, WASM < 5MB gzip
