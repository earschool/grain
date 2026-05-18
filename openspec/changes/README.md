# OpenSpec Changes — Grain

Each subdirectory is one proposed change. Structure:

```
changes/
├── <change-id>/
│   ├── proposal.md   # What and why
│   ├── design.md     # How (technical decisions)
│   └── tasks.md      # Step-by-step implementation
```

## Change Inventory

| ID | Feature | Status | Priority |
|---|---|---|---|
| 001-upload-flow | Upload flow + async job queue | Draft | P0 |
| 002-analysis-pipeline | DSP analysis pipeline (5-panel + JSON) | Draft | P0 |
| 003-auth | Magic link auth | Draft | P0 |
| 004-credit-system | Credit system + Stripe | Draft | P0 |
| 005-interactive-spectrogram | Interactive spectrogram (click/lasso + annotations) | Draft | P1 |
| 006-demucs-separation | Demucs source separation (membership-only) | Draft | P1 |
| 007-comparison-view | Comparison view (before/after overlay) | Draft | P2 |
| 008-ai-guide-mode | AI guide mode (chart region annotations) | Draft | P2 |
| 009-ai-explain-mode | AI explain mode (written summary) | Draft | P2 |
| 010-human-review-marketplace | Human review marketplace + reviewer portal | Draft | P3 |
| 011-shader-risograph | SkSL risograph shader (Flutter/Impeller) | Draft | P3 |
| 012-3d-animation | three_dart 3D logo animation | Draft | P3 |
| 013-membership-tiers | Membership tiers (Creator/Pro/Studio) | Draft | P1 |

## Process

1. Each change starts as a `proposal.md` stub
2. Design discussion fills `design.md`
3. Implementation tasks go in `tasks.md`
4. On approval, content moves to `openspec/specs/`
