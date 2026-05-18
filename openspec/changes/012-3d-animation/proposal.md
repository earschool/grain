# Change 012: three_dart 3D Logo Animation

**Status:** Draft stub
**Priority:** P3
**Phase:** 3 (visual polish)

## What

three_dart (Dart port of Three.js) 3D animation for Grain logo and landing page. Inflated 3D G, waveform spur on horizontal bar, shadow stream (5 flat G silhouettes offset horizontal).

## Technical Reference

See `knowledge/engineering/shader-architecture.md` §3 for three_dart implementation sketch.

## Acceptance Criteria (stub)

- [ ] three_dart renders in Flutter (iOS, Android, Web)
- [ ] Inflated 3D G with amber-gold material
- [ ] Waveform spur animates
- [ ] Shadow stream: 5 silhouettes, decreasing opacity
- [ ] Gentle rotation animation
- [ ] No compositing issues (runs in Impeller pipeline)
