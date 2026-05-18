# Grain — Requirements Specification

**Document Status:** Draft — stub, to be filled during OpenSpec phase
**Version:** 0.1.0
**Date:** 2026-05-19
**Author:** Ear School
**Product:** Grain (audio analysis micro-SaaS)
**Domain:** grain.audio

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Goals and Objectives](#2-goals-and-objectives)
3. [User Stories and Use Cases](#3-user-stories-and-use-cases)
4. [Functional Requirements](#4-functional-requirements)
5. [Non-Functional Requirements](#5-non-functional-requirements)
6. [Technical Requirements](#6-technical-requirements)
7. [Design Considerations](#7-design-considerations)
8. [Testing and Quality Assurance](#8-testing-and-quality-assurance)
9. [Deployment and Release](#9-deployment-and-release)
10. [Maintenance and Support](#10-maintenance-and-support)
11. [Future Enhancements](#11-future-enhancements)
12. [Stakeholder Information](#12-stakeholder-information)
13. [Open Decisions](#13-open-decisions)

---

## 1. Introduction

### 1.1 Purpose

This document defines the requirements for **Grain**, an audio analysis micro-SaaS built by Ear School. Grain accepts uploaded audio recordings and produces signal-level charts: spectrogram, pitch track, vibrato width, RMS dynamics, and spectral brightness. Users can optionally add an AI interpretation layer and access a human expert review marketplace.

This document is the authoritative specification for Grain's scope, features, and constraints.

### 1.2 Scope

**In scope:**
- Audio file upload (MP3/WAV/AAC/FLAC, up to ~10 min)
- 5-panel DSP chart generation (free tier): spectrogram, pitch, vibrato width, dynamics, brightness
- Interactive spectrogram: click/lasso region → timestamp → annotation panel
- Annotation system: notes, tags, timestamps, shareable, exportable
- Source separation via Demucs (membership-only)
- Comparison view: overlay two runs of same song (paid)
- AI guide mode: LLM annotates chart regions (paid)
- AI explain mode: plain-language written summary (paid)
- Credit system with pay-as-you-go and membership tiers
- Human review marketplace: async critique from vetted professionals
- Stripe payments (credits + subscriptions)
- Auth (magic link)
- Risograph visual identity (SkSL shaders via Flutter/Impeller)
- 3D animation (three_dart)

**Out of scope:**
- Real-time analysis (mic input, live monitoring) — future consideration
- DAW plugin or desktop app
- Music theory curriculum or educational content
- Social features (follows, public profiles) — future consideration
- Mobile-native apps (Phase 0 is web-first Flutter)

### 1.3 Intended Audience

| Audience | How they use this document |
|---|---|
| Developers | Implementation reference, acceptance criteria |
| Designers | UI/UX requirements, visual identity |
| QA | Test case derivation |
| Product | Priority validation, scope agreement |
| Reviewers (marketplace) | Understanding of reviewer portal features |

### 1.4 Glossary

| Term | Definition |
|---|---|
| Chart set | The 5-panel DSP output: spectrogram + pitch + vibrato + dynamics + brightness |
| Stem | An isolated audio track from source separation (vocals, drums, bass, etc.) |
| Credit | Unit of account for Grain operations. 50 credits = $5.00 |
| Voiced frame | Audio frame where pitch is detected (pYIN voiced_flag = true) |
| pYIN | Probabilistic YIN pitch detection algorithm (librosa implementation) |
| Demucs | Facebook Research source separation model (htdemucs family) |
| Guide mode | AI mode that annotates chart regions with detected events |
| Explain mode | AI mode that produces a plain-language written summary of charts |

---

## 2. Goals and Objectives

### 2.1 Product Goals

1. **Honest signal data.** Deliver objective, measurement-based analysis. No AI scores, no opaque "grades." pyin doesn't have opinions.
2. **Consumer-accessible.** First analysis micro-SaaS with web upload, pYIN pitch tracking, and per-frame vibrato width accessible to non-technical musicians.
3. **Layered value.** Free DSP charts → paid AI interpretation → premium human review. Multiple ways to monetize the same upload.
4. **Visual identity differentiation.** Risograph aesthetic via SkSL shaders — looks nothing like any competitor.

### 2.2 Business Goals

1. Reach $X MRR within N months of public launch (to be defined).
2. Onboard 5 founding reviewers before public launch.
3. Build community presence in r/singing, r/classicalmusic, NATS, vocal Discord servers.

---

## 3. User Stories and Use Cases

### 3.1 Personas

| Persona | Who | Pain | WTP |
|---|---|---|---|
| Classical/opera singer | Conservatory students, pre-pro 18–45 | Can't tell if vibrato improving between lessons | $15–25/mo or $10–20/analysis |
| Musical theatre singer | 16–35, audition-driven | Needs data not feelings before auditions | $10–20/mo |
| Indie/singer-songwriter | 20–40, home studio | Can hear "something's off" but can't diagnose | $10–20/analysis |
| Vocal coach (B2B) | Independent teacher, 5–30 students | No data between lessons; student progress invisible | $40–60/mo (Studio tier) |
| Music student | University vocal student 18–24 | Show progress to professors; jury recordings | Low individually; university licensing |

### 3.2 Core User Stories

<!-- Expand each story with acceptance criteria during OpenSpec phase -->

| ID | As a... | I want to... | So that... |
|---|---|---|---|
| US-001 | singer | upload an audio file and see pitch, vibrato, dynamics charts | I can understand my performance objectively |
| US-002 | singer | annotate a region of the spectrogram | I can mark moments to revisit |
| US-003 | singer | compare before/after recordings | I can see improvement over time |
| US-004 | vocal coach | share an annotated chart with my student | I can reference signal data during lessons |
| US-005 | user | separate my vocal from backing track | I can analyze just my voice from a home recording |
| US-006 | user | get an AI explanation of my charts | I can understand what the data means |
| US-007 | user | get an AI annotation on chart regions | I can see which moments are significant |
| US-008 | user | request a critique from a professional reviewer | I can get expert human feedback |
| US-009 | reviewer | receive an audio file + charts and submit a critique | I can provide signal-anchored feedback |
| US-010 | user | buy credits or subscribe | I can access paid features |

---

## 4. Functional Requirements

<!-- Each requirement should be fleshed out with acceptance criteria in child OpenSpec changes -->

### 4.1 Upload Flow

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-001 | Accept MP3, WAV, AAC, FLAC files up to ~10 min / ~200 MB | P0 |
| REQ-GRN-002 | Drag-drop + file picker upload UI | P0 |
| REQ-GRN-003 | Show upload progress + job queue position | P0 |
| REQ-GRN-004 | Return analysis results page when job completes | P0 |
| REQ-GRN-005 | Store uploaded files securely with user association | P0 |

### 4.2 DSP Analysis (Chart Generation)

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-010 | Generate mel spectrogram (dB scale, dark theme) | P0 |
| REQ-GRN-011 | Generate pYIN pitch track with confidence coloring (plasma colormap) | P0 |
| REQ-GRN-012 | Generate per-frame vibrato width (rolling std dev over ~0.25s window) | P0 |
| REQ-GRN-013 | Generate RMS dynamics (librosa feature.rms, dB) | P0 |
| REQ-GRN-014 | Generate spectral brightness (spectral centroid) | P0 |
| REQ-GRN-015 | Chart generation is 0 credits (always free) | P0 |
| REQ-GRN-016 | Output downloadable 5-panel PNG | P0 |
| REQ-GRN-017 | Output JSON data arrays for interactive frontend rendering | P1 |

### 4.3 Interactive Spectrogram + Annotations

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-020 | Click or lasso-select a region on any chart panel | P1 |
| REQ-GRN-021 | Selected region highlights across all panels simultaneously | P1 |
| REQ-GRN-022 | Annotation panel: free-text note + tag selector + timestamps | P1 |
| REQ-GRN-023 | Annotations saved per analysis, retrievable on revisit | P1 |
| REQ-GRN-024 | Tags: user-defined + preset library (pitch break, vibrato kicks in, etc.) | P1 |
| REQ-GRN-025 | Annotations exportable as timestamped CSV or PDF | P2 |
| REQ-GRN-026 | Annotations shareable (coach → student link) | P2 |

### 4.4 Source Separation (Demucs)

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-030 | Demucs separation available to members only (not pay-as-you-go) | P1 |
| REQ-GRN-031 | Model selector: htdemucs / htdemucs_ft / htdemucs_6s | P1 |
| REQ-GRN-032 | Stem selector: vocals / drums / bass / other (+ guitar, piano for 6s) | P1 |
| REQ-GRN-033 | Shifts slider: 1–10 (quality vs speed) | P1 |
| REQ-GRN-034 | Output format: WAV or MP3 | P1 |
| REQ-GRN-035 | Separation job runs async; member sees queue position | P1 |
| REQ-GRN-036 | All extracted stems available for download | P1 |
| REQ-GRN-037 | User selects which stem to pipe into analysis (default: vocals) | P1 |
| REQ-GRN-038 | Separation credit costs: 2-stem=10cr, 4-stem=20cr, 6-stem=30cr, ft=+15cr | P1 |

### 4.5 Comparison View

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-040 | Upload same song twice (before/after) and overlay pitch tracks | P2 |
| REQ-GRN-041 | Comparison view is paid (membership-gated) | P2 |
| REQ-GRN-042 | Visual diff highlighting between runs | P2 |

### 4.6 AI Guide Mode

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-050 | LLM annotates chart regions with detected events (paid) | P2 |
| REQ-GRN-051 | Uses chart data as input (not raw audio) — deterministic, auditable | P2 |
| REQ-GRN-052 | Must declare assumptions: genre/style context stated explicitly | P2 |
| REQ-GRN-053 | Highlights regions: "vibrato emerges here", "pitch droops on phrase", etc. | P2 |
| REQ-GRN-054 | Credit cost: 20cr (Sonnet) | P2 |

### 4.7 AI Explain Mode

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-060 | LLM produces plain-language written explanation of charts (paid) | P2 |
| REQ-GRN-061 | Observational framing — what the charts show, not judgment | P2 |
| REQ-GRN-062 | Assumptions stated explicitly | P2 |
| REQ-GRN-063 | Credit tiers: 5cr (Haiku/summary), 20cr (Sonnet/deep), 50cr (Opus/expert) | P2 |

### 4.8 Credit System

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-070 | Credit anchor: 50 credits = $5.00 | P0 |
| REQ-GRN-071 | Pay-as-you-go bundles: 50cr/$5, 200cr/$16, 500cr/$35 | P0 |
| REQ-GRN-072 | Free tier: 10 credits/month, no payment required | P0 |
| REQ-GRN-073 | Membership tiers: Creator (100cr/$8), Pro (300cr/$20), Studio (1000cr/$50) | P1 |
| REQ-GRN-074 | Membership: included credits + discounted overage + advanced feature access | P1 |
| REQ-GRN-075 | Stripe integration for bundles and subscriptions | P0 |
| REQ-GRN-076 | Credit balance visible at all times | P0 |

### 4.9 Human Review Marketplace

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-080 | Reviewer sets own price freely (suggested floor, no ceiling) | P3 |
| REQ-GRN-081 | Platform takes 20–25% commission (TBD) | P3 |
| REQ-GRN-082 | Reviewer portal: receive file + charts, submit written + optional voice critique | P3 |
| REQ-GRN-083 | Founding reviewers: Jake Chapman, Gene Coye, Sophie Lagan | P3 |
| REQ-GRN-084 | Rating system for reviewers; SLA enforcement | P3 |
| REQ-GRN-085 | Onboarding messaging: "Your expertise is rare. Price accordingly." | P3 |

### 4.10 Auth

| ID | Requirement | Priority |
|---|---|---|
| REQ-GRN-090 | Magic link auth (preferred) or email/password | P0 |
| REQ-GRN-091 | Session persistence; no re-auth per upload | P0 |

---

## 5. Non-Functional Requirements

| ID | Requirement | Target |
|---|---|---|
| NFR-GRN-001 | Chart generation time (3-min file, no separation) | < 30s |
| NFR-GRN-002 | Upload handling | Async; no blocking UI |
| NFR-GRN-003 | Demucs separation (4-stem, htdemucs, shifts=1, 3-min) | < 5 min |
| NFR-GRN-004 | Chart PNG download | < 2s after analysis complete |
| NFR-GRN-005 | HTTPS everywhere | Required |
| NFR-GRN-006 | Audio files stored securely; access-controlled by user | Required |
| NFR-GRN-007 | Graceful degradation if SkSL shaders fail | CSS fallback |
| NFR-GRN-008 | Mobile-responsive web UI | Required |

---

## 6. Technical Requirements

| ID | Requirement |
|---|---|
| TR-GRN-001 | Python 3.11+ for analysis backend |
| TR-GRN-002 | librosa: pyin, feature.rms, melspectrogram, spectral_centroid |
| TR-GRN-003 | Demucs: htdemucs, htdemucs_ft, htdemucs_6s |
| TR-GRN-004 | FastAPI for API layer |
| TR-GRN-005 | Flutter (Dart) for frontend |
| TR-GRN-006 | SkSL for risograph visual shaders |
| TR-GRN-007 | three_dart for 3D animation |
| TR-GRN-008 | Stripe for payments |
| TR-GRN-009 | JSON output from analysis for interactive chart rendering |
| TR-GRN-010 | ffmpeg for audio decode (AAC, FLAC pre-processing) |

---

## 7. Design Considerations

### 7.1 Visual Identity

Grain uses the Ear School risograph visual system: paper texture, overprint effects, halftone patterns, misregistration feel. Amber-gold accent color differentiates from Cadence (blue) and Riso (TBD).

**Shader strategy:**
- Phase 1–2: CSS-approximated risograph textures (paper grain, muted palette) — no SKSL dependency
- Phase 3: Full SkSL shader integration via Flutter/Impeller pipeline
- SkSL shaders must not block functional delivery

### 7.2 Chart Design Principles

- Dark theme (dark background, high-contrast chart lines)
- Confidence coloring on pitch track (plasma colormap, voiced_prob)
- NaN gaps in vibrato track where audio is unvoiced (no false smoothing)
- Chart panels synchronized: selecting a time region highlights across all panels

### 7.3 3D Animation

- Logo animation + landing page motion: three_dart (Dart port of Three.js)
- Runs in same Flutter pipeline as UI — no platform view compositing issues
- Inflated 3D G logo with waveform spur, shadow stream effect

---

## 8. Testing and Quality Assurance

<!-- Stub — expand during OpenSpec phase -->

| Area | Approach |
|---|---|
| Analysis pipeline | Unit tests on known audio fixtures (verify pYIN output, RMS values) |
| Credit system | Integration tests: purchase, deduction, membership gates |
| Upload flow | E2E: upload → job queue → chart result |
| Demucs separation | Smoke test: upload known mix, verify stems returned |
| Auth | Magic link flow; session expiry |
| Stripe | Webhook handling in test mode |

---

## 9. Deployment and Release

<!-- Stub — hosting decision pending -->

- **Hosting:** TBD (Fly.io / Railway / Render)
- **CI/CD:** GitHub Actions
- **Environments:** dev, staging, prod
- **Secrets management:** TBD
- **Domain:** grain.audio (Alice, Namecheap — pending)

---

## 10. Maintenance and Support

<!-- Stub -->

- Analysis library updates: monitor librosa releases for pYIN accuracy improvements
- Demucs model updates: htdemucs family updated periodically by Facebook Research
- Credit pricing: Alice can adjust; no code change for rate table if externalized to config

---

## 11. Future Enhancements

- Real-time analysis (mic input)
- Mobile-native Flutter app (iOS/Android)
- Social: public analysis sharing, community annotations
- University/institutional licensing
- Chromascope integration: pitch-class analysis of vocal melody
- Cadence integration: voice-read analysis output

---

## 12. Stakeholder Information

| Stakeholder | Role |
|---|---|
| Alice | Co-founder, product owner, brand, design |
| Aphra | Co-founder, technical advisor |
| Jake Chapman | Founding reviewer candidate |
| Gene Coye | Founding reviewer candidate |
| Sophie Lagan | Founding reviewer candidate |

---

## 13. Open Decisions

| # | Decision | Options | Owner |
|---|---|---|---|
| 1 | Hosting platform | Fly.io / Railway / Render | Alice |
| 2 | Auth method | Magic link vs email/password | Alice |
| 3 | Marketplace commission | 20% or 25% | Alice |
| 4 | Interactive charts in MVP? | Phase 1 or Phase 2 | Alice |
| 5 | Demucs hosting | Fly.io GPU instance vs external | TBD |
| 6 | Stripe account | Alice to create | Alice |
| 7 | grain.audio domain | Alice to purchase (Namecheap) | Alice |
