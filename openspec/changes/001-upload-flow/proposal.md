# Change 001: Upload Flow + Async Job Queue

**Status:** Draft stub
**Priority:** P0
**Phase:** 1 (MVP)

## What

File upload endpoint that accepts MP3/WAV/AAC/FLAC (up to ~10 min / ~200 MB), queues an analysis job, returns job ID, and notifies client on completion.

## Why

Upload and job queue is the entry point to everything in Grain. No other feature exists without this.

## Scope

- Drag-drop + file picker UI (Flutter)
- FastAPI upload endpoint (multipart/form-data)
- File validation: type, size, duration
- Async job queue (job ID returned immediately)
- Job status polling or WebSocket notification
- File storage (local in dev, cloud in prod)
- Job result: analysis results URL

## Out of Scope

- Audio analysis itself (see 002-analysis-pipeline)
- Source separation (see 006-demucs-separation)
- Credit deduction (see 004-credit-system — but upload itself is free/0 credits)

## Open Questions

- Job queue implementation: FastAPI BackgroundTasks vs Celery vs ARQ?
- File storage: local filesystem vs S3-compatible vs fly.io volumes?
- Max file size: hard limit or soft limit with warning?
- Duration detection: ffprobe before queue or let analysis fail?

## Acceptance Criteria

- [ ] MP3/WAV/AAC/FLAC accepted; other types rejected with error message
- [ ] Files > 200 MB rejected
- [ ] Job ID returned within 2s of upload
- [ ] Client can poll /jobs/{id} for status
- [ ] On completion, client redirected to analysis results
- [ ] Uploaded files associated with authenticated user
- [ ] Upload progress visible in UI
