# Grain — Parallelization Analysis

**Document Status:** Stub — to be expanded when build team is assembled
**Date:** 2026-05-19
**Input documents:** `requirements.md` v0.1.0, `priorities.md` v0.1.0, `roadmap.md`
**Product:** Grain (audio analysis micro-SaaS)

---

## 1. Dependency Graph

### Internal Dependency Map

```
                   ┌─────────────────────────┐
                   │  PHASE 1: FOUNDATION     │
                   │  Upload + DSP + PNG       │
                   │  JSON output pipeline     │
                   │  FastAPI skeleton         │
                   └────────────┬────────────┘
                                │
              ┌─────────────────┼──────────────┐
              │                 │              │
              ▼                 ▼              ▼
   ┌─────────────────┐  ┌──────────────┐  ┌────────────────┐
   │ CREDITS + AUTH   │  │ FLUTTER      │  │ ANALYSIS       │
   │ STREAM           │  │ FRONTEND     │  │ PIPELINE       │
   │                  │  │ STREAM       │  │ STREAM         │
   │ - Credit system  │  │ - App shell  │  │ - Demucs       │
   │ - Stripe         │  │ - Upload UI  │  │   integration  │
   │ - Magic link     │  │ - Chart view │  │ - JSON output  │
   │ - Free tier      │  │ - SkSL stub  │  │ - Job queue    │
   └────────┬─────────┘  └──────┬───────┘  └───────┬────────┘
            │                   │                   │
            └─────────┬─────────┘                   │
                      │                             │
                      ▼                             ▼
         ┌────────────────────────┐    ┌─────────────────────┐
         │ PHASE 3: INTERACTIVE   │    │ PHASE 4: MEMBERSHIP  │
         │ Annotations, Comparison│    │ Demucs + Member tiers│
         └────────────┬───────────┘    └──────────┬──────────┘
                      │                           │
                      └────────────┬──────────────┘
                                   │
                                   ▼
                      ┌─────────────────────────┐
                      │ PHASE 5: AI LAYER        │
                      │ Guide mode, Explain mode │
                      └────────────┬────────────┘
                                   │
                                   ▼
                      ┌─────────────────────────┐
                      │ PHASE 6: MARKETPLACE     │
                      │ Reviewer portal, Reviews │
                      └─────────────────────────┘
```

---

## 2. Parallel Streams

### Stream A: Backend / Analysis Pipeline

**Owner:** Backend agent
**Blocks:** Everything

| Phase | Tasks |
|---|---|
| 1 | FastAPI skeleton, upload endpoint, async job queue, librosa pipeline (5-panel), PNG output |
| 1 | JSON data output endpoint (pitch array, spectrogram matrix, rms, brightness) |
| 2 | Credit system DB model, deduction logic, free tier |
| 2 | Stripe webhook handlers (payment intent, subscription events) |
| 4 | Demucs integration (htdemucs family, async separation job) |
| 5 | AI guide/explain endpoints (chart data → Claude Haiku/Sonnet/Opus) |

### Stream B: Flutter Frontend

**Owner:** Frontend agent
**Depends on:** Stream A JSON endpoints (can be stubbed)

| Phase | Tasks |
|---|---|
| 1 | Flutter project setup, routing, app shell |
| 1 | Upload flow UI (drag-drop, progress, job queue position) |
| 1 | Chart display (Canvas 2D rendering from JSON data) |
| 2 | Auth (magic link flow) |
| 2 | Credit balance display, purchase flow |
| 3 | Interactive spectrogram (click/lasso, cross-panel sync) |
| 3 | Annotation panel (notes, tags, timestamps, export) |
| 3 | Comparison view (overlay two pitch tracks) |
| 7 | Full SkSL risograph shader integration |

### Stream C: Visual Identity / Shaders

**Owner:** Shader/design agent (or frontend agent)
**Can start after:** Flutter app shell exists

| Phase | Tasks |
|---|---|
| 1–2 | CSS-approximated risograph textures (paper grain, muted palette, halftone) |
| Research | SkSL risograph technique — paper texture, overprint simulation, misregistration |
| Research | three_dart 3D logo animation (inflated G, waveform spur, shadow stream) |
| 7 | Full SkSL shader implementation (replace CSS approximations) |

### Stream D: Marketplace / Reviewer Portal

**Owner:** Marketplace agent (future)
**Depends on:** Auth, credit system, analysis pipeline complete

| Phase | Tasks |
|---|---|
| 6 | Reviewer onboarding portal |
| 6 | Review request flow (upload + charts delivered to reviewer) |
| 6 | Reviewer submission UI (written + optional voice critique) |
| 6 | Commission handling (Stripe Connect or manual payout) |
| 6 | Rating system, SLA enforcement |

---

## 3. Agent Team (Phase 1–2, Solo)

Current state: single developer / single agent team. Parallelism is sequential phases with independent stream unblocking.

When build team scales:
- **Backend agent:** Streams A (analysis pipeline, API, job queue)
- **Frontend agent:** Streams B + C early phases (Flutter, CSS shaders)
- **Shader agent:** Stream C Phase 7 (full SkSL)
- **Marketplace agent:** Stream D

---

## 4. External Dependencies

| Dependency | Owner | Unblocks |
|---|---|---|
| grain.audio domain | Alice (Namecheap) | Landing page, SSL certs |
| Stripe account | Alice | All paid features |
| Grain logo final | Alice (Illustrator) | Visual identity, marketing |
| Hosting platform decision | Alice | Deployment pipeline |
| Demucs GPU hosting | TBD | Phase 4 Demucs |

---

## 5. Notes

- **Spec before code.** Full OpenSpec pass on each stream before implementation.
- **CSS shaders from day one.** Do not ship a generic-looking admin panel. Paper texture and grain via CSS is the minimum bar.
- **JSON pipeline is the architectural unlock.** Everything interactive depends on it. Prioritize early.
