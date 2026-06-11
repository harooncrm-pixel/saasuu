# Project Scaffold: OmniDoc (Codename)

> Build this folder structure on Day 1. Every path below is a concrete directory or file to create.

## Root Layout

```
omnidoc/
├── README.md                          # Product one-liner, badges, build status
├── CONTRIBUTING.md                    # Contributor guide (open-source core)
├── LICENSE                            # BSL 1.1 (core) + commercial license
├── SECURITY.md                        # Security policy & disclosure process
├── ARCHITECTURE.md                    # Copied/adapted from STRATEGIC_PLAN.md §6
│
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                     # Run on PR: lint, test, build
│   │   ├── release.yml                # Tag → publish packages
│   │   └── e2e.yml                    # Playwright E2E suite
│   ├── CODEOWNERS                     # Team ownership per path
│   └── PULL_REQUEST_TEMPLATE.md
│
├── docs/
│   ├── adr/                           # Architecture Decision Records
│   │   └── 0001-use-wasm-pdf-engine.md
│   ├── specs/                         # Detailed feature specs
│   │   ├── 001-pdf-viewer.md
│   │   ├── 002-ai-chat.md
│   │   └── 003-user-auth.md
│   ├── api/                           # OpenAPI 3.1 spec
│   │   └── openapi.yaml
│   └── product/                       # Product/market artifacts
│       └── prd.md                     # Copy of STRATEGIC_PLAN.md §1-5
│
├── packages/
│   ├── pdf-engine/                    # Rust → WASM PDF renderer
│   │   ├── Cargo.toml
│   │   ├── src/
│   │   │   ├── lib.rs                 # Public API surface
│   │   │   ├── renderer/             # PDFium-based tile renderer
│   │   │   ├── text/                 # Text extraction, editing
│   │   │   ├── annotation/           # Comments, highlights, stamps
│   │   │   └── wasm/                 # WASM bindings (wasm-bindgen)
│   │   ├── tests/
│   │   └── benches/
│   │
│   ├── pdf-engine-js/                 # NPM wrapper for wasm package
│   │   ├── package.json
│   │   ├── src/
│   │   │   ├── index.ts
│   │   │   └── worker.ts              # Off-main-thread WASM
│   │   └── __tests__/
│   │
│   ├── shared-types/                  # TypeScript types shared across packages
│   │   ├── package.json
│   │   └── src/
│   │       ├── document.ts
│   │       ├── annotation.ts
│   │       ├── user.ts
│   │       └── index.ts
│   │
│   └── ai-client/                     # Unified AI provider client
│       ├── package.json
│       └── src/
│           ├── providers/
│           │   ├── openai.ts
│           │   ├── anthropic.ts
│           │   ├── llama-cpp.ts        # Local inference
│           │   └── index.ts
│           ├── router.ts               # Tiered model routing
│           ├── types.ts
│           └── index.ts
│
├── apps/
│   ├── web/                           # Next.js web app
│   │   ├── package.json
│   │   ├── next.config.ts
│   │   ├── tailwind.config.ts
│   │   ├── tsconfig.json
│   │   ├── public/
│   │   │   ├── favicon.ico
│   │   │   └── wasm/                  # PDF engine WASM assets
│   │   ├── src/
│   │   │   ├── app/                   # Next.js App Router pages
│   │   │   │   ├── layout.tsx
│   │   │   │   ├── page.tsx           # Landing / file picker
│   │   │   │   ├── editor/
│   │   │   │   │   └── [id]/
│   │   │   │   │       └── page.tsx   # PDF editor view
│   │   │   │   ├── api/               # API routes (thin proxies to backend)
│   │   │   │   │   ├── auth/
│   │   │   │   │   ├── documents/
│   │   │   │   │   └── ai/
│   │   │   │   ├── pricing/
│   │   │   │   └── sign/
│   │   │   ├── components/
│   │   │   │   ├── editor/            # PDF editor UI components
│   │   │   │   ├── ai/                # AI chat, smart redact, etc.
│   │   │   │   ├── ui/                # Shared primitives (Radix-based)
│   │   │   │   └── layout/            # Shell layout, sidebar, toolbar
│   │   │   ├── hooks/                 # React hooks
│   │   │   ├── stores/                # Zustand stores
│   │   │   ├── lib/                   # Utilities, API client
│   │   │   └── styles/
│   │   │       └── globals.css
│   │   ├── e2e/
│   │   └── vitest.config.ts
│   │
│   ├── desktop/                       # Tauri desktop app (optional Phase 1)
│   │   ├── src-tauri/
│   │   │   ├── Cargo.toml
│   │   │   ├── src/
│   │   │   └── tauri.conf.json
│   │   └── src/
│   │
│   └── mobile/                        # Flutter mobile app (Phase 2)
│       └── (placeholder)
│
├── services/                          # Backend microservices
│   ├── api-gateway/                   # Kong/Envoy config
│   │   └── kong.yaml
│   │
│   ├── auth/                          # Auth service (Ory Kratos + OAuth2 Proxy)
│   │   ├── docker-compose.yml
│   │   └── kratos-config.yml
│   │
│   ├── document-service/              # Rust (Actix/Tokio)
│   │   ├── Cargo.toml
│   │   ├── src/
│   │   │   ├── main.rs
│   │   │   ├── api/
│   │   │   ├── storage/               # S3-compatible object store
│   │   │   ├── conversion/            # Format conversion pipeline
│   │   │   └── db/                    # PostgreSQL access layer
│   │   ├── tests/
│   │   └── Dockerfile
│   │
│   ├── ai-service/                    # Rust + ONNX Runtime / llama.cpp
│   │   ├── Cargo.toml
│   │   ├── src/
│   │   │   ├── main.rs
│   │   │   ├── ocr/                  # Vision-transformer OCR
│   │   │   ├── llm/                  # LLM inference orchestration
│   │   │   ├── classify/             # Document classification
│   │   │   ├── extract/              # Structured data extraction
│   │   │   └── models/               # Model loading & caching
│   │   ├── tests/
│   │   └── Dockerfile
│   │
│   ├── esign-service/                 # Rust + custom PKI
│   │   ├── Cargo.toml
│   │   └── src/
│   │
│   └── workflow-engine/               # Temporal.io worker
│       ├── Cargo.toml
│       └── src/
│
├── infrastructure/                    # IaC & deployment
│   ├── k8s/
│   │   ├── namespaces/
│   │   ├── services/
│   │   └── helm/                     # Helm charts for self-hosted
│   ├── terraform/
│   │   ├── aws/
│   │   └── cloudflare/
│   ├── docker/
│   │   ├── docker-compose.dev.yml
│   │   └── docker-compose.prod.yml
│   └── scripts/
│       ├── setup-dev.sh              # One-command dev environment
│       └── seed-demo-data.sh
│
├── tooling/
│   ├── eslint.config.mjs
│   ├── tsconfig.base.json
│   ├── vitest.workspace.ts
│   └── .prettierrc
│
└── package.json                       # Root workspace (npm workspaces)
```

## First Files to Create (Priority Order)

| # | File | Why First |
|---|------|-----------|
| 1 | `packages/pdf-engine/Cargo.toml` | Core technical moat — start WASM PDF rendering immediately |
| 2 | `packages/pdf-engine-js/package.json` | JS wrapper for the WASM engine — consume in web app |
| 3 | `apps/web/next.config.ts` | Web app scaffold — usable standalone |
| 4 | `services/document-service/Cargo.toml` | Document CRUD + storage API |
| 5 | `infrastructure/docker/docker-compose.dev.yml` | One-command dev startup (PostgreSQL, MinIO, Redis) |
| 6 | `docs/specs/001-pdf-viewer.md` | First detailed feature spec — get the team aligned |
| 7 | `docs/adr/0001-use-wasm-pdf-engine.md` | First ADR — lock in the most consequential decision |

## Workspace Configuration (package.json root)

```json
{
  "name": "omnidoc",
  "private": true,
  "workspaces": [
    "packages/*",
    "apps/*",
    "services/*"
  ],
  "scripts": {
    "dev": "npm run dev --workspace=apps/web",
    "build": "npm run build --workspaces",
    "test": "npm run test --workspaces",
    "lint": "eslint .",
    "setup": "bash infrastructure/scripts/setup-dev.sh"
  }
}
```
