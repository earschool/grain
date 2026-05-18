# Change 006: Demucs Source Separation

**Status:** Draft stub
**Priority:** P1
**Phase:** 2 (Interactive Charts / Membership)

## What

Membership-only feature to separate uploaded audio into stems (vocals, drums, bass, other, etc.) using Facebook Research's Demucs model family. User selects which stem to pipe into the analysis pipeline.

## Why

Musicians recording at home have backing tracks mixed in. Analyzing vocals over a full mix produces noisy pitch/vibrato readings. Demucs isolation produces much cleaner analysis of the vocal stem.

## Models

| Model | Stems | Quality |
|---|---|---|
| htdemucs | vocals, drums, bass, other | Default, fast |
| htdemucs_ft | vocals, drums, bass, other | Fine-tuned, higher quality |
| htdemucs_6s | vocals, drums, bass, guitar, piano, other | 6 stems |

## Credit Costs (Locked)

| Operation | Credits |
|---|---|
| 2-stem separation (htdemucs, vocals only) | 10 |
| 4-stem separation (htdemucs) | 20 |
| 6-stem separation (htdemucs_6s) | 30 |
| Fine-tuned (+htdemucs_ft) | +15 on top |

## Settings UI

- Model selector: htdemucs / htdemucs_ft / htdemucs_6s
- Stems to extract (checkboxes): vocals / drums / bass / other / guitar / piano
- Shifts slider: 1–10 (higher = better quality, slower)
- Output format: WAV or MP3

## Flow

1. Upload mixed recording
2. Select separation settings (above)
3. Separation job queued (async; member sees queue position + priority)
4. All extracted stems available for download
5. User selects which stem to pipe into analysis (default: vocals)
6. Analysis runs on selected stem → cleaner charts

## Membership Gate

- Separation only available to members (Creator/Pro/Studio)
- Pay-as-you-go users see feature preview + "upgrade to access"
- Demucs credits deducted from member's balance (same credit system)

## Open Questions

- Hosting: same Fly.io instance as FastAPI, or separate GPU instance?
- GPU requirement: Demucs can run on CPU (slower) or GPU (fast). CPU viable for MVP?
- Priority queue: members jump ahead of free-tier jobs (for any operations, not just separation)

## Acceptance Criteria

- [ ] Separation feature hidden/locked for non-members
- [ ] Member can select model, stems, shifts, output format
- [ ] Separation job queued + async; progress visible
- [ ] All stems available for download on completion
- [ ] User can select stem to pipe into analysis
- [ ] Analysis on selected stem produces cleaner pitch/vibrato charts
- [ ] Credits deducted per credit table above
- [ ] Fine-tuned model charges additional 15cr
