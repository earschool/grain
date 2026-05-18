# Change 008: AI Guide Mode

**Status:** Draft stub
**Priority:** P2
**Phase:** 3

## What

LLM annotates chart regions directly with detected events. Uses chart data (JSON arrays) as input, not raw audio. Output: overlay annotations on charts.

## Why

Chart data is objective but requires interpretation skill. Guide mode lowers the barrier for users who can see the charts but don't know what to look for.

## Key Design Rule (Locked)

LLM must declare assumptions explicitly: "In folk/classical context, vibrato rate X Hz is [typical/unusual]." No opaque grades.

## Credit Cost

20 credits (Sonnet). Deep analysis tier.

## Acceptance Criteria (stub)

- [ ] LLM receives JSON chart data (not audio)
- [ ] Returns region annotations with time ranges and descriptions
- [ ] Annotations overlaid on charts
- [ ] Genre/style assumption stated in output
- [ ] 20 credits deducted
