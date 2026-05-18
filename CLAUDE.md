# CLAUDE.md — Grain by Ear School

## What This Is

Grain is an audio analysis micro-SaaS by Ear School (ear.school). Upload any recording — vocals, instruments, full mix — receive signal-level charts: spectrogram, pitch track, vibrato width, RMS dynamics, spectral brightness. Honest, unbiased feedback. Measurements, not judgments.

**One-liner:** "You already know your voice. Now see it."
**Domain:** grain.audio
**Repo:** earschool/grain

## Key Design Commitments (Locked)

- Signal processing output = objective measurements. pyin has no opinions. RMS doesn't know what "good dynamics" means.
- Any LLM interpretation must declare genre/style assumptions explicitly.
- Free tier: raw charts always free, no credits. Downloadable PNG = free marketing.
- Analysis is general-purpose (not vocals-only). Vocal isolation is optional pre-step.
- Spectrogram always included — anchor visual of the product.
- Name: Grain — audio grain + paper grain. Amber-gold accent. grain.audio.

## Repository Structure

```
openspec/         — Change proposals and feature specs
  changes/        — One dir per feature/change (proposal.md, design.md, tasks.md)
  specs/          — Canonical capability specs
knowledge/
  strategy/       — Product strategy, vision, competitive landscape
  research/       — Technical research (shaders, DSP, etc.)
plans/            — Roadmap, priorities, phase breakdown
src/
  backend/        — FastAPI Python backend
  frontend/       — Flutter frontend
```

## Tech Stack

| Layer | Choice | Notes |
|-------|--------|-------|
| Analysis | Python + librosa | pyin, feature.rms, melspectrogram, spectral_centroid |
| Vocal isolation | audio-separator v0.44.1 | UVR-MDX-NET-Inst_HQ_3.onnx; optional pre-step |
| Backend | FastAPI | Python native; async job queue |
| Frontend | Flutter | SkSL shaders out of the box; no DOM; Impeller pipeline |
| Shader | SkSL (Flutter) | Risograph effect — paper grain, overprint, misregistration |
| 3D animation | three_dart | Path spline + material + lighting in same Flutter pipeline |
| Payments | Stripe | Credits + subscription |
| Auth | TBD | Magic link recommended |
| Hosting | TBD | Fly.io / Railway / Render |
| AI layer | Claude Haiku/Sonnet/Opus | Chart-data-in → interpretation-out; tiered by credit |

## Credit System

| Bundle | Credits | Price |
|--------|---------|-------|
| Free tier | 3/month | $0 |
| Starter | 10 | $1.00 |
| Standard | 60 | $5.00 |
| Pro | 250 | $15.00 |
| Studio | 1,000 | $50.00 |
| Subscription | 200/mo | $15/mo |

| Operation | Credits | Model |
|-----------|---------|-------|
| Chart generation | 0 | DSP only |
| Basic AI analysis | 1 | Haiku |
| Deep analysis | 3 | Sonnet |
| Expert analysis | 10 | Opus |

## Dev Rules

- TDD mandatory
- 300-line file threshold
- Small PRs
- No rebase on main
- OpenSpec gates all significant features — no code before spec is approved

## Open Decisions

1. Hosting — Fly.io / Railway / Render
2. Auth — magic link vs email/password
3. three_dart vs platform view for 3D animation layer
4. Marketplace commission — 20% or 25%

## Working Prototype

Analysis script at `/workspace/agent/analysis/audio_analysis.py` (in Knack's agent workspace, not this repo). 5-panel output: spectrogram + confidence-colored pitch + vibrato width + dynamics + brightness.
