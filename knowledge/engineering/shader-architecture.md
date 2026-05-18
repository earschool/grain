# Grain — Shader Architecture Research

**Last updated:** 2026-05-19
**Status:** Research notes — not yet implemented

---

## Overview

Grain uses the Ear School risograph visual identity. This document covers:
1. How to implement risograph effects in SkSL (Flutter/Impeller)
2. three_dart for 3D animation (logo, landing page)
3. Decision rationale (why not Three.js / WebGL)

---

## 1. Risograph Visual Identity — What We're Simulating

Risograph printing characteristics to simulate:
- **Paper texture** — irregular grain/tooth, slightly warm off-white background
- **Ink overprint** — where two ink layers overlap, colors multiply (not blend)
- **Halftone dots** — visible dot screen at close range, raster feel
- **Misregistration** — slight offset between color layers (~1–3px drift)
- **Ink bleed** — soft edge where ink saturates paper fibers
- **Limited palette** — typically 2–3 flat colors, no gradients within a layer

Ear School's specific palette: amber-gold accent (Grain), with muted earthy secondary tones. Dark mode base.

---

## 2. SkSL (Skia Shading Language)

### What it is
SkSL is Skia's shading language — a dialect of GLSL that compiles to GLSL ES 1.0, GLSL, HLSL, MSL, SPIR-V, or Skia's own bytecode depending on backend. Flutter uses Skia (and Impeller, the new renderer) — both support SkSL fragment shaders via `FragmentProgram` API.

### Flutter FragmentProgram API

```dart
// Load and compile shader at startup
final program = await FragmentProgram.fromAsset('shaders/risograph.frag');
final shader = program.fragmentShader();

// Set uniforms
shader.setFloat(0, time);        // uniform float uTime
shader.setFloat(1, grainAmount); // uniform float uGrain
shader.setImageSampler(0, image); // uniform sampler2D uTexture

// Paint with shader
final paint = Paint()..shader = shader;
canvas.drawRect(rect, paint);
```

Shaders must be declared in `pubspec.yaml`:
```yaml
flutter:
  shaders:
    - shaders/risograph.frag
```

### Risograph Fragment Shader Techniques

#### Paper texture (grain noise)
```glsl
// Simplex or value noise for paper grain
float grain(vec2 uv, float scale) {
    vec2 p = floor(uv * scale);
    vec2 f = fract(uv * scale);
    float n = dot(p, vec2(127.1, 311.7));
    return fract(sin(n) * 43758.5453);
}

// Apply to fragment
float paperGrain = grain(uv, 512.0) * 0.05; // subtle
fragColor = baseColor + vec4(paperGrain);
```

#### Halftone dots
```glsl
// Dot screen at angle
float halftone(vec2 uv, float frequency, float angle) {
    float s = sin(angle), c = cos(angle);
    vec2 rotUV = vec2(uv.x * c - uv.y * s, uv.x * s + uv.y * c);
    vec2 nearest = 2.0 * fract(rotUV * frequency) - 1.0;
    float dist = length(nearest);
    return step(dist, 0.5); // binary dot
}
```

#### Overprint multiply
```glsl
// Two ink layers multiplied (risograph overprint)
vec3 layer1 = vec3(0.95, 0.72, 0.12); // amber-gold
vec3 layer2 = vec3(0.2, 0.2, 0.3);    // dark base
vec3 overprint = layer1 * layer2;      // multiply blend
```

#### Misregistration
```glsl
// Slight UV offset for second color layer
vec2 offset = vec2(0.002, 0.001); // ~2px drift
vec4 layer2Color = texture(uTexture, uv + offset);
```

### Impeller vs Skia

Flutter's new renderer (Impeller) has partial SkSL support. Status as of 2026:
- **iOS (Impeller):** SkSL via Metal backend — generally supported
- **Android (Impeller):** SkSL via Vulkan — supported in recent Impeller versions
- **Flutter Web (Skia/CanvasKit):** SkSL supported via WebGL backend
- **Desktop (Impeller):** Support varies by platform

**Key risk:** Impeller's SkSL coverage is still maturing. Test shader compilation on all target platforms during Phase 1 tech spike. Have CSS fallback ready.

### CSS Fallback (Phase 1–2)

Until SkSL is validated, approximate risograph effect with CSS:
```css
.risograph-surface {
  background-color: #1a1814;           /* dark off-black, warm */
  background-image: url('noise.png');  /* pre-rendered grain texture */
  background-blend-mode: overlay;
  filter: contrast(1.05) saturate(0.9);
}

.accent-amber {
  color: #f2b83a;
  text-shadow: 1px 1px 0 rgba(242, 184, 58, 0.3); /* misregistration hint */
}
```

---

## 3. three_dart (3D Animation)

### What it is

`three_dart` is a Dart port of Three.js — same scene graph, materials, lights, geometries — running in Flutter via a custom render target. It runs in the same Impeller/Skia pipeline as the rest of the Flutter app, avoiding platform view compositing issues.

**GitHub:** https://github.com/Knightro63/three_dart
**Package:** `three_dart` on pub.dev

### Why three_dart over Three.js

| | three_dart | Three.js (via WebView/iframe) |
|---|---|---|
| Render pipeline | Same as Flutter (Impeller/Skia) | Separate WebGL context |
| Compositing | Native Flutter widget | Platform view — compositing issues |
| Performance | No bridge overhead | JS↔Dart bridge latency |
| Input handling | Native Flutter gestures | Need to bridge touch events |
| Platform support | iOS, Android, Web, Desktop | Web-only without extra work |

Platform view compositing in Flutter causes known issues: flicker, input latency, z-ordering problems. three_dart avoids this entirely.

### Grain Logo Animation Concept

From brief: "Inflated 3D G, waveform spur on horizontal bar, shadow stream right (5 flat G silhouettes offset purely horizontal)"

three_dart implementation sketch:
```dart
// Scene setup
final scene = three.Scene();
final camera = three.PerspectiveCamera(45, aspect, 0.1, 100);
camera.position.set(0, 0, 5);

// Inflated G geometry — extruded from SVG path
final gGeometry = ExtrudeGeometry(gShape, ExtrudeGeometryOptions(
  depth: 0.3,
  bevelEnabled: true,
  bevelThickness: 0.05,
  bevelSize: 0.05,
));

// Amber-gold material
final gMaterial = three.MeshStandardMaterial({
  'color': three.Color(0xf2b83a),
  'roughness': 0.7,   // matte paper-like
  'metalness': 0.0,
});

// Shadow stream — 5 ghost silhouettes, offset on X
for (int i = 1; i <= 5; i++) {
  final ghost = three.Mesh(gGeometry, shadowMaterial);
  ghost.position.x = i * 0.15;
  ghost.position.z = -i * 0.1;
  ghost.material.opacity = 1.0 - (i * 0.18);
  scene.add(ghost);
}

// Waveform spur — sine wave geometry on horizontal bar
// ... (LineGeometry with sin wave points)
```

### Animation Loop

three_dart integrates with Flutter's animation framework via `AnimationController`:
```dart
AnimationController _controller;

void _onFrame(Duration elapsed) {
  final t = elapsed.inMilliseconds / 1000.0;
  gMesh.rotation.y = sin(t * 0.5) * 0.2; // gentle rocking
  waveformSpur.geometry = _buildWaveform(t); // animated waveform
  renderer.render(scene, camera);
}
```

---

## 4. Architecture Decision: JSON Pipeline + Canvas 2D

The chart rendering (spectrogram, pitch, vibrato, etc.) does NOT use three_dart or SkSL shaders directly. The pipeline is:

1. **Python backend** — librosa analysis → JSON arrays (pitch[], spectrogram[][], rms[], brightness[])
2. **Flutter Canvas 2D** — reads JSON, renders charts using `canvas.drawPoints`, `canvas.drawRect` etc.
3. **SkSL shader** — runs as a post-process over the entire chart surface (applies risograph grain/texture)

This separation means:
- Chart data is pure numbers — no rendering dependency on shader availability
- SkSL shader is purely cosmetic — if it fails, charts still render correctly
- Charts are interactive (Flutter canvas handles gestures natively)

```
JSON data → Flutter Canvas 2D (charts) → SkSL post-process (risograph texture)
                                        ↑
                              Applied as paint shader over canvas output
```

---

## 5. Open Questions / Spikes Needed

| Question | How to answer |
|---|---|
| Impeller SkSL support on Flutter Web (2026) | Tech spike: compile test shader, render in browser, measure perf |
| three_dart maturity for production use | Prototype: render inflated G, verify on iOS + Android + Web |
| CSS grain texture vs procedural noise | Visual comparison: pre-rendered PNG noise vs GLSL `grain()` function |
| SkSL shader hot-reload in Flutter debug mode | Test: edit `.frag` file, hot reload, verify shader recompiles |
| Demucs GPU on same host as FastAPI | Check Fly.io GPU machine pricing vs separate GPU instance |

---

## 6. References

- Flutter FragmentProgram docs: https://api.flutter.dev/flutter/dart-ui/FragmentProgram-class.html
- Impeller SkSL status: Flutter GitHub — impeller/docs
- three_dart: https://pub.dev/packages/three_dart
- Demucs: https://github.com/adefossez/demucs
- Risograph halftone techniques: CSS-Tricks, shader toy examples
- earschool-visual (Ear School visual identity system): earschool/riso plans/earschool-visual/
