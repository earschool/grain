# Change 002: DSP Analysis Pipeline

**Status:** Draft stub
**Priority:** P0
**Phase:** 1 (MVP)

## What

Python analysis pipeline that takes an uploaded audio file and produces:
1. 5-panel chart PNG (spectrogram, pitch, vibrato, dynamics, brightness)
2. JSON data arrays for interactive frontend rendering

## Why

The chart output is Grain's core value. Free, downloadable, shareable. Everything else is built on top.

## Current State

Working prototype at `/workspace/agent/analysis/audio_analysis.py`. Produces 5-panel PNG. Needs:
- FastAPI integration (called from job worker)
- JSON output endpoint (for interactive charts)
- ffmpeg preprocessing (AAC decode)
- Error handling for edge cases (silence, very short files, corrupt audio)

## Analysis Pipeline

1. ffmpeg decode → WAV (22050 Hz, mono) [handles AAC, FLAC, MP3]
2. librosa load → numpy array
3. pYIN → f0[], voiced_flag[], voiced_prob[]
4. rolling_std(f0_midi, 11 frames) → vibrato_width[] [NaN where unvoiced]
5. librosa.feature.rms() → rms[]
6. librosa.feature.spectral_centroid() → brightness[]
7. librosa.feature.melspectrogram() → spectrogram[][]
8. matplotlib render → 5-panel PNG
9. JSON serialization → {pitch, vibrato, rms, brightness, spectrogram}

## Chart Design (Locked)

- Panel 1: Mel spectrogram (dB, dark theme, hot colormap)
- Panel 2: pYIN pitch track (confidence-colored via plasma colormap)
- Panel 3: Per-frame vibrato width (rolling std dev, NaN gaps where unvoiced)
- Panel 4: RMS dynamics (dB)
- Panel 5: Spectral brightness (spectral centroid)

## Open Questions

- JSON format: time-indexed arrays or frame-indexed?
- Spectrogram JSON: full matrix (large) or compressed (mel bins + time frames separately)?
- Cache analysis results? (same file hash → same charts)
- Analysis parameters configurable per request? (fmin/fmax, hop_length)

## Acceptance Criteria

- [ ] 5-panel PNG generated for MP3/WAV/AAC/FLAC inputs
- [ ] PNG downloadable (no credits required)
- [ ] JSON arrays returned (pitch, vibrato, rms, brightness, spectrogram)
- [ ] AAC files decoded via ffmpeg before librosa load
- [ ] Silence / very short files handled gracefully (error message, not crash)
- [ ] Analysis time < 30s for 3-min file
