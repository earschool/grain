# Grain — Priorities Analysis

**Document Status:** Stub — to be expanded after requirements review
**Date:** 2026-05-19
**Input document:** `plans/requirements.md` v0.1.0
**Product:** Grain (audio analysis micro-SaaS)

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Value Chain Analysis](#2-value-chain-analysis)
3. [Feature Prioritization Matrix](#3-feature-prioritization-matrix)
4. [RICE Scoring](#4-rice-scoring)
5. [Recommended Build Phases](#5-recommended-build-phases)
6. [Critical Path Analysis](#6-critical-path-analysis)
7. [Risk Assessment](#7-risk-assessment)
8. [Quick Wins](#8-quick-wins)
9. [Strategic Recommendations](#9-strategic-recommendations)
10. [Open Questions Impact Analysis](#10-open-questions-impact-analysis)

---

## 1. Executive Summary

### Strategic Priority

Grain's north star: **a musician uploads any audio file and receives objective signal-level feedback in under 30 seconds, for free.** Every prioritization decision should be evaluated against this north star. The 5-panel chart PNG is the minimum viable product. If it is missing, slow, or broken, nothing else matters.

### Key Findings

1. **Chart generation is the gateway to everything.** REQ-GRN-001 through REQ-GRN-016 (upload + DSP analysis + PNG output) must be correct and fast before any paid features are built. Free charts = free marketing = viral loop.

2. **The JSON data pipeline (REQ-GRN-017) is the architectural unlock for interactive charts.** Without it, the interactive spectrogram, comparison view, and AI guide mode cannot be built. This is P1 but should be sequenced immediately after the PNG pipeline.

3. **SkSL shaders are a strategic differentiator but a delivery risk.** Grain must not look like a generic SaaS. The risograph visual identity is non-negotiable. However, full SkSL integration should be phased: CSS-approximated textures for Phase 1–2, full shaders in Phase 3.

4. **Demucs is a meaningful membership differentiator.** Source separation gives subscribers a concrete capability (not just more credits) that non-subscribers can't access. It is correctly gated to membership.

5. **The human review marketplace is Phase 3+ scope.** It requires reviewer onboarding, portal, rating system, SLA enforcement — significant work. It should not block public launch. Founding reviewer relationships (Jake, Gene, Sophie) should be cultivated in parallel, not gated on marketplace completion.

6. **AI features depend on chart data quality.** Guide mode and explain mode take chart data as input, not raw audio. They can only be as good as the DSP pipeline. Build and validate the pipeline first.

### Recommended Build Order

| Phase | Content | Notes |
|---|---|---|
| **Phase 1: Foundation** | Upload + async job queue + 5-panel DSP + PNG download + JSON output | Gate to everything else |
| **Phase 2: Credits + Auth + Stripe** | Credit system, magic link auth, pay-as-you-go bundles, free tier | Monetization foundation |
| **Phase 3: Interactive Charts** | Interactive spectrogram, annotations, comparison view | Core UX differentiator |
| **Phase 4: Membership + Demucs** | Membership tiers, Demucs separation, stem downloads | Subscription revenue |
| **Phase 5: AI Layer** | Guide mode, explain mode | LLM-powered paid features |
| **Phase 6: Marketplace** | Reviewer portal, review request flow, rating system | Human expert layer |
| **Phase 7: Visual Polish** | Full SkSL shader integration | Replaces CSS approximations |

---

## 2. Value Chain Analysis

<!-- Stub — fill with dependency mapping -->

Core value chain:

```
Upload → DSP Analysis → Chart Output (free)
                     → AI Interpretation (paid)
                     → Human Review (paid, marketplace)
```

Each layer adds value to the same uploaded artifact. Credits and membership gate the upper layers.

---

## 3. Feature Prioritization Matrix

<!-- Stub — expand with full requirement list scored against value / effort / risk -->

| Feature | Value | Effort | Risk | Priority |
|---|---|---|---|---|
| Upload + DSP + PNG | High | Low | Low | P0 |
| JSON data output | High | Low | Low | P1 |
| Credit system | High | Medium | Low | P0 |
| Auth (magic link) | High | Low | Low | P0 |
| Stripe integration | High | Medium | Medium | P0 |
| Interactive spectrogram | High | High | Medium | P1 |
| Annotations | High | Medium | Low | P1 |
| Demucs separation | High | High | Medium | P1 |
| Comparison view | Medium | Medium | Low | P2 |
| AI guide mode | Medium | Medium | Low | P2 |
| AI explain mode | Medium | Low | Low | P2 |
| Human review marketplace | High | Very High | Medium | P3 |
| Full SkSL shaders | Medium | High | High | P3 |
| 3D logo animation | Low | Medium | Low | P3 |

---

## 4. RICE Scoring

<!-- Stub — fill with Reach × Impact × Confidence / Effort scores -->

*(To be completed during OpenSpec phase)*

---

## 5. Recommended Build Phases

See Executive Summary Section 1 for phase table.

Phase gate conditions:

- **Phase 1 → Phase 2:** Charts generate correctly for MP3/WAV/AAC/FLAC. PNG download works. JSON output is validated.
- **Phase 2 → Phase 3:** Free tier credits working. Stripe accepting test payments. Auth flow complete.
- **Phase 3 → Phase 4:** Interactive spectrogram working. Annotations persist and export.
- **Phase 4 → Phase 5:** Demucs running correctly. Membership gating enforced.
- **Phase 5 → Phase 6:** AI guide and explain modes in production. Credit deduction working.
- **Phase 6 → Phase 7:** Marketplace accepting first real reviews. Rating system live.

---

## 6. Critical Path Analysis

<!-- Stub -->

Critical path to public launch (Product Hunt):
1. Upload + DSP + PNG (P0)
2. Auth + Credits + Stripe (P0)
3. Waitlist landing at grain.audio (can be a static page — unblock on domain purchase)
4. Beta: invite waitlist, free analyses, 5 vocal coaches for testimonials
5. Public launch

Interactive spectrogram and Demucs separation are not required for launch. They are the post-launch growth drivers.

---

## 7. Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| SkSL shader performance on Flutter Web | Medium | Medium | CSS textures as fallback; decouple shader work from functional delivery |
| Demucs GPU hosting cost | Medium | High | Membership-only gates the feature; credit cost covers infrastructure |
| pYIN accuracy on degraded audio | Low | Medium | Document known limitations; test against diverse file types |
| Reviewer supply for marketplace | High | High | Cultivate Jake/Gene/Sophie before launch; do not gate launch on marketplace |
| Stripe setup delay (Alice action item) | Medium | High | Can build credit UI against test mode; switch to live when Alice sets up account |

---

## 8. Quick Wins

1. **Static landing page at grain.audio** — before any feature is built, a waitlist page with a sample chart PNG demonstrates the product and captures emails.
2. **5-panel PNG shareable on social** — the chart PNG is free to download. Watermark with grain.audio. Every shared chart is a free impression.
3. **Community posts with sample charts** — "What does a vibrato problem actually look like?" posts in r/singing with real charts drive organic traffic before launch.

---

## 9. Strategic Recommendations

<!-- Stub — to be expanded -->

1. **Launch with free charts and capture emails first.** The product's unique value (objective signal data) is demonstrable in a static chart. Don't wait for interactive features to start marketing.
2. **Demucs as membership hook.** The separation capability is a concrete, visible feature that justifies membership. Market it as "analyze your voice, not the backing track."
3. **Reviewers as credibility anchors.** Even before marketplace is live, having Gene Coye or Jake Chapman's name attached to the product signals legitimacy in music communities.
4. **Price discovery through marketplace.** Let reviewers set high prices early. This signals that Grain takes expert feedback seriously and establishes premium positioning.

---

## 10. Open Questions Impact Analysis

| Question | Impact if deferred | Recommendation |
|---|---|---|
| Interactive charts in MVP? | Low — PNG download works as MVP; interactive is P1 growth feature | Defer to Phase 3 |
| Hosting platform | Medium — affects Demucs GPU availability and cost | Decide before Phase 4 build |
| Marketplace commission (20% vs 25%) | Low pre-marketplace | Decide before Phase 6 build |
| Stripe account (Alice) | High — blocks all paid features | Unblock ASAP |
