# Change 005: Interactive Spectrogram + Annotations

**Status:** Draft stub
**Priority:** P1
**Phase:** 2 (Interactive Charts)

## What

Clicking or lasso-selecting a region of any chart panel highlights that time range across all panels and opens an annotation panel for notes, tags, and timestamps.

## Why

Grain's key differentiator vs static chart tools (VoceVista, Sing & See). Turns analysis into a conversation — coach marks moments for student, singer marks problem phrases, reviewer anchors critique to signal data.

## Annotation Data Model (Locked)

```
Annotation {
  id: uuid
  analysis_id: uuid
  t_start: float    // seconds
  t_end: float      // seconds
  panel: enum       // spectrogram | pitch | dynamics | brightness
  note: string
  tags: string[]
  created_at: timestamp
}
```

## Scope

- Click → point annotation (t_start = t_end)
- Lasso/drag → range annotation
- Selected region highlights across all 5 panels simultaneously
- Annotation panel: free-text note, tag selector, timestamps
- Tags: preset library + user-defined
- Annotations saved per analysis (authenticated user)
- Annotations visible as overlay markers on chart
- Annotations exportable (CSV or PDF with chart thumbnails)
- Sharing: coach can share annotated analysis link with student

## Preset Tag Library (stub — expand)

`pitch break`, `vibrato kicks in`, `breath support drops`, `dynamic peak`, `formant shift`, `pitch droops`, `vibrato inconsistent`, `nice phrase`, `needs work`

## Open Questions

- Annotation panel: inline (beside chart) or modal overlay?
- Cross-panel sync: same pixel-time mapping across all panels? (Yes, if JSON timeline is shared)
- Mobile: touch select via long press + drag?
- Sharing: copy link vs invite by email?

## Acceptance Criteria

- [ ] Click any panel → point annotation at that time
- [ ] Drag any panel → range annotation for that time range
- [ ] Selection highlighted across all 5 panels
- [ ] Annotation panel opens with time pre-filled
- [ ] Note text saved on submit
- [ ] Tags (preset + custom) attachable to annotation
- [ ] Annotations persist across page refresh
- [ ] Annotations visible as markers on chart
- [ ] Export: annotations + timestamps to CSV
- [ ] Shareable link to annotated analysis (read-only for recipient)
