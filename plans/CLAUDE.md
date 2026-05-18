# CLAUDE.md — grain

## Product Description

Grain is an audio analysis micro-SaaS. Upload any recording — vocals, instrument, full mix — receive signal-level charts: spectrogram, pitch track, vibrato width, RMS dynamics, spectral brightness. Honest, unbiased feedback. Measurements, not judgments.

**One-liner:** "You already know your voice. Now see it."

**GitHub:** `github.com/earschool/grain`
**Domain:** grain.audio
**Requirement Prefix:** REQ-GRN-NNN

## Current Phase: Phase 0 (Foundation)

Phase 0 tasks: domain purchase (Alice), logo final (Alice), Stripe setup (Alice), full OpenSpec pass on all features.

**First build task:** FastAPI backend + async job queue + upload flow (Phase 1 MVP, once spec is complete).

## Technology Stack

### Backend
- **Language:** Python 3.11+
- **Framework:** FastAPI
- **Analysis:** librosa (pyin, feature.rms, melspectrogram, spectral_centroid)
- **Source separation:** Demucs (htdemucs, htdemucs_ft, htdemucs_6s) — membership-only
- **Job queue:** async (Celery or FastAPI BackgroundTasks TBD)
- **Payments:** Stripe (credits + subscriptions)

### Frontend
- **Framework:** Flutter (Dart)
- **Shaders:** SkSL (risograph visual identity — paper texture, overprint, grain)
- **3D animation:** three_dart
- **Rendering pipeline:** Impeller

### Infrastructure
- **Hosting:** TBD (Fly.io / Railway / Render)
- **Auth:** Magic link (recommended) or email/password

## Key Design Decisions (Locked)

1. **Signal = data, not judgment.** pyin doesn't have opinions about vibrato. RMS doesn't know what "good dynamics" means. LLM interpretation must declare its genre/style assumptions explicitly.
2. **Free tier: raw charts always free.** DSP chart generation = 0 credits. Downloadable PNG = free marketing.
3. **ElevenLabs membership model.** Monthly credits included + discounted rate + advanced feature access.
4. **Credit anchor: 50 credits = $5.** Average 3-minute full expert (Opus) analysis = 50 credits = $5.
5. **Demucs is membership-only.** Separation is compute-heavy; membership pricing absorbs cost + gates capability.
6. **Reviewer-set prices.** Platform encourages premium pricing. "Your expertise is rare. Price accordingly."

## Architecture Decisions

1. **JSON output pipeline.** Python backend outputs raw arrays as JSON. Flutter canvas renders charts. SkSL shader post-processes.
2. **Spec before code.** Full OpenSpec pass on all features before any implementation work.
3. **Flutter + SkSL native.** No DOM, no WebGL bridging — risograph shader runs in same Impeller pipeline as UI.
4. **three_dart not Three.js.** Stays in Flutter pipeline, avoids platform view compositing issues.

## Key Directories

```
grain/
├── plans/              # Requirements, priorities, roadmap, parallelization
├── openspec/
│   ├── specs/          # Source-of-truth specs (post-approval)
│   └── changes/        # Change proposals (proposal.md, design.md, tasks.md)
├── knowledge/
│   ├── strategy/       # Competitive analysis, ICP, GTM, positioning
│   ├── research/       # Technical research, spike results
│   └── engineering/    # Architectural decisions, shader research
├── src/
│   ├── backend/        # FastAPI + analysis pipeline
│   └── frontend/       # Flutter app
└── CLAUDE.md           # This file
```
