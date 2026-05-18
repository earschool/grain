# Change 011: SkSL Risograph Shader

**Status:** Draft stub
**Priority:** P3
**Phase:** 3 (visual polish)

## What

Full SkSL risograph shader implementation via Flutter/Impeller. Replaces CSS-approximated textures with procedural grain, overprint simulation, halftone patterns, misregistration effect.

## Why

Risograph visual identity is Grain's design differentiator. CSS approximations ship earlier; SkSL is the full expression.

## Technical Reference

See `knowledge/engineering/shader-architecture.md` for full implementation notes.

## Acceptance Criteria (stub)

- [ ] SkSL shader compiles and runs on iOS, Android, Web (Flutter)
- [ ] Paper grain effect visible on surfaces
- [ ] Amber-gold overprint effect on accent elements
- [ ] Halftone dots visible at appropriate zoom
- [ ] CSS fallback when SkSL unavailable
- [ ] No functional regression when shader disabled
