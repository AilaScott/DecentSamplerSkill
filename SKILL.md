---
name: decent-sampler
description: Use when creating, editing, validating, or cleaning .dspreset files for DecentSampler. Covers XML structure, UI controls, effects, oscillators (wavetable, FM6, pluck, harmonic), modulators, buses, note sequences, keyswitches, and MPE.
license: MIT
---

# DecentSampler .dspreset File Reference

DecentSampler is a free, open-source sample instrument player by Dave Hilowitz. Each instrument is defined by a single `.dspreset` XML file plus a folder of assets (audio files, images, wavetables).  Many .dspresets and sample folders can be zipped inside a parent directory and then loaded from DecentSampler as a library of many presets.

## What is a DecentSampler Instrument?

A DecentSampler instrument consists of:
1. A `.dspreset` file (XML) — the preset definition (required)
2. A folder of assets (optional) — audio files, images, wavetables

Typical folder structure:
```
MyInstrument/
├── MyInstrument.dspreset    ← Preset definition (XML)
├── Samples/                 ← Audio files (.wav, .aif)
│   ├── C3.wav
│   ├── C4.wav
│   └── ...
├── Images/                  ← UI images (optional)
│   ├── Background.jpg
│   └── Knob.png
└── Wavetables/              ← Wavetable files (optional)
    └── MyWavetable.wav
```

All paths in the .dspreset file are **relative to the .dspreset file location**. The .dspreset file is the only required file; audio files are referenced by path.

## File Format Overview

Every `.dspreset` file is XML. The structure:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<DecentSampler minVersion="1.0.0">
  <tags>...</tags>
  <ui width="812" height="375">...</ui>
  <groups>...</groups>
  <effects>...</effects>
  <modulators>...</modulators>
  <buses>...</buses>
  <midi>...</midi>
  <noteSequences>...</noteSequences>
</DecentSampler>
```

All child elements of `<DecentSampler>` are optional except that at least one `<groups>` with samples/oscillators is needed for sound.

---

## Root Element: `<DecentSampler>`

| Attribute | Required | Description |
|-----------|----------|-------------|
| minVersion | No | Minimum DecentSampler version (e.g., "1.19.0") |

### minVersion Strategy

Use the highest minVersion required by your features:

| Feature | Required minVersion |
|---------|---------------------|
| Basic samples, UI, effects | 1.0.0 |
| Keyswitches | 1.4.0 |
| Colored keyboard keys | 1.4.0 |
| Modulators (LFO, envelope) | 1.6.0 |
| Wave folder/shaper effects | 1.7.2 |
| Note sequences | 1.11.1 |
| Buses | 1.12.0 |
| FM6 oscillators | 1.12.0 |
| Pitch shift effect | 1.13.3 |
| Oscillators (basic) | 1.15.0 |
| Wavetable oscillators | 1.15.0 |
| Animations | 1.12.9 |
| Stereo simulator | 1.17.0 |
| Bit crusher | 1.7.2 |

Rule: If in doubt, use `minVersion="1.19.0"` for modern features or `minVersion="1.0.0"` for basic presets.

---

## Color Format

All colors use 8-digit ARGB hex: `AARRGGBB`

- `FF000000` = solid black
- `80FF0000` = 50% transparent red
- `FF4A90E2` = solid blue
- Use `#` prefix only in bgColor attribute: `bgColor="#FF1A1A2E"`

---

## The `<ui>` Element

Defines the user interface. Recommended dimensions: **812 × 375**.

| Attribute | Required | Description |
|-----------|----------|-------------|
| width | Yes | UI width in pixels (recommended: 812) |
| height | Yes | UI height in pixels (recommended: 375) |
| bgColor | No | 8-digit ARGB background color |
| bgImage | No | Path to background image (relative to .dspreset) |
| coverArt | No | Cover art for "My Libraries" tab |
| layoutMode | No | "relative" for responsive layouts |

### The `<tab>` Element

One `<tab>` per UI. Contains all controls.

| Attribute | Required | Description |
|-----------|----------|-------------|
| name | No | Tab name (not displayed) |

### UI Controls

#### `<labeled-knob>` / `<control>`

Rotary dials, sliders, or custom-skinned controls. `<labeled-knob>` has a built-in label; `<control>` does not.

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position (0,0 = top-left) |
| width | Yes | Width in pixels |
| height | No | Height in pixels |
| label | No | Text label (labeled-knob only) |
| parameterName | Yes | Name shown in shrunken UI |
| type | No | "float" (default), "integer", "multi_state", "musical_time" |
| minValue | No | Minimum value (default: 0) |
| maxValue | No | Maximum value (default: 1) |
| value | No | Initial value (default: 0) |
| defaultValue | No | Value on double-click |
| textColor | No | 8-digit ARGB |
| textSize | No | Font size (default: 12) |
| trackForegroundColor | No | Knob track fill color |
| trackBackgroundColor | No | Knob track background color |
| style | No | "rotary" (default), "linear_bar", "linear_bar_vertical", "rotary_vertical_drag", "custom_skin_vertical_drag" |
| showLabel | No | Show/hide built-in label (true for labeled-knob, false for control) |
| snapMode | No | Value snapping: "none" (default), "whole_numbers", "tenths", "hundredths", "thousandths", "stop_points" |
| snapStopPoints | No | Comma-separated values for snapMode="stop_points" |
| defeatSnapWithShift | No | Allow shift key to bypass snap (true/false) |
| mouseDragSensitivity | No | Integer, higher = less sensitive |
| uid | No | Unique identifier for the control |
| visible | No | true/false |
| enabled | No | true/false |
| tags | No | Comma-separated tags |
| tooltip | No | Hover tooltip |
| customSkinImage | No | Path to KnobMan image |
| customSkinNumFrames | No | Frame count for custom skin |
| customSkinImageOrientation | No | "horizontal" or "vertical" |

**Note on UI control indexing:** All UI elements (including `<label>`, `<image>`, etc.) are indexed in order. If your tab has `<label>`, `<control>`, `<label>`, `<control>`, the control positions are 1 and 3 (0-indexed).

#### `<menu>` — Dropdown Menu

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position |
| width, height | Yes | Dimensions |
| value | No | 1-based index of selected option (default: 0 = none) |
| textColor | No | Text color ARGB |
| backgroundColor | No | Background color ARGB |
| highlightedTextColor | No | Selected text color |
| highlightedBackgroundColor | No | Selected background color |
| vAlign | No | "top", "center" (default), "bottom" |
| hAlign | No | "left" (default), "center", "right" |
| requireSelection | No | If true, user must select an option (true/false) |
| placeholderText | No | Text shown when no option selected |
| visible, enabled | No | true/false |
| tags | No | Comma-separated tags |

Contains `<option>` children with `name` attribute and `<binding>` elements.

**Important:** Menu option indices are **1-based**. `value="1"` selects the first option.

**Dynamic color control:** Menu colors can be controlled via bindings using parameters: `TEXT_COLOR`, `BACKGROUND_COLOR`, `HIGHLIGHTED_TEXT_COLOR`, `HIGHLIGHTED_BACKGROUND_COLOR`.

#### `<button>` — Text or Image Button

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position |
| width, height | Yes | Dimensions |
| style | No | "text" (default) or "image" |
| value | No | 0-based index of active state |
| mainImage | For images | Main button image |
| hoverImage | No | Hover state image |
| clickImage | No | Click state image |
| visible, enabled | No | true/false |
| tags | No | Comma-separated tags |

Contains `<state>` children with `name` attribute and `<binding>` elements.

#### `<label>` — Static Text

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position |
| width, height | Yes | Dimensions |
| text | Yes | Text to display |
| textColor | No | 8-digit ARGB |
| textSize | No | Font size (default: 12) |
| vAlign | No | "top", "center" (default), "bottom" |
| hAlign | No | "left", "center" (default), "right" |
| visible | No | true/false |
| tags | No | Comma-separated tags |

#### `<image>` — Static Image

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position |
| width, height | Yes | Dimensions |
| path | Yes | Image file path |
| aspectRatioMode | No | "preserve" or "stretch" |
| opacity | No | 0.0–1.0 (default: 1) |
| visible | No | true/false |

#### `<rectangle>` — Filled Rectangle

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position |
| width, height | Yes | Dimensions |
| fillColor | No | 8-digit ARGB |
| borderColor | No | 8-digit ARGB |
| borderThickness | No | Pixels (0 = none) |
| visible | No | true/false |

#### `<line>` — Line Segment

| Attribute | Required | Description |
|-----------|----------|-------------|
| x1, y1 | Yes | Start point |
| x2, y2 | Yes | End point |
| lineColor | No | 8-digit ARGB |
| lineThickness | No | Pixels (default: 1.0) |
| visible | No | true/false |

#### `<xyPad>` — 2D Pad Control

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position |
| width, height | Yes | Dimensions |
| markerDiameter | No | Marker size (default: 10) |
| markerFillColor | No | Marker fill ARGB |
| outlineColor | No | Outline ARGB |
| bgColor | No | Background ARGB |
| xValue, yValue | No | Initial values (0.0) |
| visible, enabled | No | true/false |
| tags | No | Comma-separated tags |

Contains `<x>` and `<y>` children with `<binding>` elements.

#### `<multiFrameImage>` — Animation

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position |
| width, height | Yes | Dimensions |
| path | Yes | Image strip path |
| numFrames | Yes | Frame count |
| sourceFormat | Yes | "horizontal_image_strip" or "vertical_image_strip" |
| frameRate | Yes | FPS (max 24) |
| playbackMode | No | "forward_loop" (default), "forward_once", "reverse_loop", "ping_pong_loop", "stopped" |
| opacity | No | 0.0–1.0 |
| visible | No | true/false |
| tags | No | Comma-separated tags |

#### `<oscilloscope>` — Live Waveform Display

| Attribute | Required | Description |
|-----------|----------|-------------|
| x, y | Yes | Position |
| width, height | Yes | Dimensions |
| backgroundColor | No | ARGB (default: #FF000000) |
| waveColor | No | ARGB (default: #FF00FF00) |
| lineThickness | No | Pixels (default: 1.5) |
| showCenterLine | No | true/false |
| visible | No | true/false |

### The `<keyboard>` Element

Contains `<color>` children to color key ranges.

```xml
<keyboard>
  <color loNote="36" hiNote="50" color="FF2C365E"/>
  <color loNote="51" hiNote="57" color="FF6D9DC5"/>
</keyboard>
```

Colors are overlaid at 75% transparency on white keys.

---

## The `<groups>` and `<group>` Elements

Groups contain samples and/or oscillators. `<groups>` is the container; `<group>` defines individual groups.

### `<groups>` Attributes

| Attribute | Description |
|-----------|-------------|
| attack | Global attack time (seconds) |
| decay | Global decay time (seconds) |
| sustain | Global sustain level (0.0–1.0) |
| release | Global release time (seconds) |
| volume | Global volume (e.g., "-3dB" or "0.8") |
| ampVelTrack | Velocity sensitivity (0.0–1.0) |
| glideTime | Portamento time (seconds) |
| glideMode | "always", "legato", or "off" |
| seqMode | Round-robin mode: "round_robin", "random", "true_random", "always" |
| seqLength | Round-robin sequence length |

### `<group>` Attributes

All `<groups>` attributes plus:

| Attribute | Description |
|-----------|-------------|
| name | Group name |
| tags | Comma-separated tags |
| enabled | true/false (can be toggled via bindings) |
| pan | Stereo position (-100 to 100) |
| groupTuning | Pitch offset in semitones (e.g., "-12" for octave down) |
| loNote, hiNote | MIDI note range |
| loVel, hiVel | Velocity range |
| output1Target–output8Target | Audio routing (MAIN_OUTPUT, BUS_1–16, AUX_STEREO_OUTPUT_1–16) |
| output1Volume–output8Volume | Output volumes (0.0–1.0) |
| trigger | When to play: "attack" (default), "first" (only if no notes playing), "legato" (only if notes playing), "release" (on note-off), "continuous" (always) |
| silencedByTags | Comma-separated tags that silence this group when triggered |
| silencingMode | "fast" (immediate) or "normal" (triggers release) |
| silencingDecay | Custom fade-out time in seconds (overrides silencingMode) |
| loCCN / hiCCN | CC number N (1-127) that gates this group (e.g., loCC64="0" hiCC64="63") |
| onLoCCN / onHiCCN | CC number N that triggers this group (e.g., onLoCC64="90" onHiCC64="127") |
| retriggerEnabled | Enable pattern retriggering (true/false) |
| retriggerInterval | Time between retriggers (default: 4.0) |
| retriggerIntervalUnit | "seconds", "beats" (default), "samples" |
| loopEnabled | Enable looping for all samples in group (true/false) |

#### Group-Level Effects

Only these effects work at group level: `lowpass`, `highpass`, `bandpass`, `gain`, `chorus`. Reverb and delay cannot be group-level.

```xml
<group name="Pad">
  <sample path="Samples/C3.wav" rootNote="60" loNote="59" hiNote="61"/>
  <effects>
    <effect type="lowpass" frequency="2000"/>
  </effects>
</group>
```

### `<sample>` Element

| Attribute | Required | Description |
|-----------|----------|-------------|
| path | Yes | Audio file path (relative to .dspreset) |
| rootNote | Yes | MIDI note for root pitch |
| loNote | Yes | Lowest MIDI note that triggers this sample |
| hiNote | Yes | Highest MIDI note that triggers this sample |
| loVel | No | Lowest velocity (default: 0) |
| hiVel | No | Highest velocity (default: 127) |
| name | No | Sample name |
| volume | No | Volume (e.g., "-8.3dB" or "0.5") |
| pan | No | Stereo position (-100 to 100) |
| start | No | Start position in samples |
| end | No | End position in samples |
| attack | No | Per-sample attack (seconds) |
| decay | No | Per-sample decay (seconds) |
| sustain | No | Per-sample sustain (0.0–1.0) |
| release | No | Per-sample release (seconds) |
| attackCurve, decayCurve, releaseCurve | No | Curve shapes (-100 to 100) |
| loopEnabled | No | true/false |
| loopStart | No | Loop start in samples |
| loopEnd | No | Loop end in samples |
| loopCrossfade | No | Crossfade length in samples |
| loopCrossfadeMode | No | "linear" or "equal_power" |
| seqMode | No | Round-robin mode |
| seqPosition | No | Position in round-robin queue |
| seqLength | No | Round-robin sequence length |
| tags | No | Comma-separated tags |
| ampVelTrack | No | Velocity sensitivity (0.0–1.0) |
| pitchKeyTrack | No | Pitch tracking (0.0–1.0, default: 1.0) |
| playbackMode | No | "memory", "disk_streaming", "auto" |
| silencedByTags | No | Tags that silence this sample when triggered |
| silencingMode | No | "fast" (immediate) or "normal" (triggers release) |
| silencingDecay | No | Custom fade-out time in seconds (overrides silencingMode) |
| trigger | No | "attack" (default), "first", "legato", "release", "continuous" |
| previousNotes | No | Comma-separated MIDI note numbers. Only play if previous note matches one of these. |
| previousNote | No | Single MIDI note number (alias for previousNotes with one value) |
| legatoInterval | No | Semitone distance from previous note for legato trigger |
| delay | No | Delay before sample starts (seconds, beats, or samples) |
| delayUnit | No | "seconds" (default), "beats", "samples" |
| retriggerEnabled | No | Enable pattern retriggering (true/false) |
| retriggerInterval | No | Time between retriggers (default: 4.0) |
| retriggerIntervalUnit | No | "seconds", "beats" (default), "samples" |
| releaseTriggerDecay | No | Decay for release-triggered samples. Format: "3dB" (dB per second) or linear (0.0–1.0) |
| loCCN / hiCCN | No | CC number N that gates this sample (e.g., loCC64="0" hiCC64="63") |
| onLoCCN / onHiCCN | No | CC number N that triggers this sample (e.g., onLoCC64="90" onHiCC64="127") |
| output1Target–output8Target | No | Audio routing |

### The `<oscillator>` Element

Lives inside `<group>`. Generates synthetic waveforms.

| Attribute | Required | Description |
|-----------|----------|-------------|
| waveform | Yes | "sine", "saw", "square", "triangle", "noise"/"white_noise", "pluck1", "wavetable", "harmonic", "fm6op" |

**Wavetable attributes:**
| Attribute | Description |
|-----------|-------------|
| wavetableFile | Path to multi-frame .wav file |
| wavetableFrameSize | Samples per frame (default: 2048, auto-detected from Serum format) |
| wavetablePosition | Initial position (0.0–1.0) |
| randomPhase | Randomize start phase (recommended for unison) |
| wavetableFrameInterpolation | Smooth crossfade between frames (default: true) |

**Pluck attributes:**
| Attribute | Description |
|-----------|-------------|
| damping | Decay time (0.0–1.0, default: 0.5) |
| pluckType | Excitation blend (0.0=smooth, 1.0=bright, default: 0.5) |

**FM6 attributes (set on `<group>`, not `<oscillator>`):**

FM6 is a 6-operator FM synthesizer implementing the 32 classic Yamaha DX7 algorithm topologies. Each operator has its own frequency ratio, level, feedback, detune, and envelope.

| Attribute | Description |
|-----------|-------------|
| fmAlgorithm | DX7 algorithm number (1–32) |
| fmOp1Ratio–fmOp6Ratio | Frequency ratios (default: 1.0). 1.0 = fundamental, 2.0 = octave up, 0.5 = octave down |
| fmOp1Level–fmOp6Level | Output/modulation levels (0.0–1.0) |
| fmOp1Feedback–fmOp6Feedback | Self-feedback (0.0–1.0). Only Op6 feedback works in all algorithms |
| fmOp1Detune–fmOp6Detune | Pitch detune (-7 to +7). DX7-compatible: larger offsets at lower notes |
| fmOp1Mode–fmOp6Mode | "ratio" (default) or "fixed" |
| fmOp1FixedFreq–fmOp6FixedFreq | Fixed frequency in Hz (when mode="fixed") |
| fmOp1VelocitySensitivity–fmOp6VelocitySensitivity | Velocity response (0–7, DX7 convention). 0=none, 7=maximum |
| fmOp1Attack–fmOp6Attack | Per-operator ADSR attack (0–45 seconds). -1 = no independent envelope |
| fmOp1Decay–fmOp6Decay | Per-operator ADSR decay (0–45 seconds) |
| fmOp1Sustain–fmOp6Sustain | Per-operator ADSR sustain (0.0–1.0) |
| fmOp1Release–fmOp6Release | Per-operator ADSR release (0–45 seconds). -1 = use group ADSR |
| fmOp1EgType–fmOp6EgType | Envelope type: "adsr" (default) or "dx7" |
| fmOp1EgRate1–fmOp6EgRate4 | DX7 rates R1–R4 (0–99). Only when EgType="dx7" |
| fmOp1EgLevel1–fmOp6EgLevel4 | DX7 levels L1–L4 (0–99). Only when EgType="dx7" |

**Algorithm categories:**

| Algorithms | Character | Use for |
|------------|-----------|---------|
| 1–8 | Deep chains (1–2 carriers) | Rich, complex FM: basses, pads, strings |
| 9–19 | Mixed chains + parallel | Balanced, versatile: pianos, plucks, bells |
| 20–28 | 3+ carriers | Bright, additive-leaning: choir, organs, leads |
| 29–32 | 4–6 carriers | Additive/organ-like: drawbar organs, rich pads |

Algorithm 32 = all 6 operators as carriers (pure additive synthesis).

**Feedback:** Only Op6 feedback (`fmOp6Feedback`) works in all 32 algorithms. Small values (0.0–0.15) add warmth; medium (0.3–0.6) create sawtooth-like waves; high (0.7–1.0) produce noise/distortion.

**Fixed frequency mode:** Set `fmOpNMode="fixed"` and `fmOpNFixedFreq="987.5"` for metallic/bell timbres with inharmonic partials.

**DX7 envelope:** Set `fmOpNEgType="dx7"` to use classic 4-stage rate/level envelope (R1–R4, L1–L4, range 0–99). When dx7 is active, the ADSR attributes are ignored for that operator.

**Harmonic attributes (set on `<group>`):**
| Attribute | Description |
|-----------|-------------|
| numPartials | Number of active partials (1–64) |
| harmonicTilt | Spectral tilt (-1.0 to 1.0) |
| harmonicOddEvenBalance | Odd/even balance (0.0=odd, 1.0=even) |
| harmonicNormalization | Loudness compensation (0.0–1.0) |
| harmonicPartial1Level–harmonicPartial64Level | Per-partial amplitudes (0.0–1.0) |

---

## The `<effects>` Element

Global effects chain. Can also live inside `<group>` for group-level effects.

```xml
<effects>
  <effect type="lowpass" frequency="22000"/>
  <effect type="reverb" wetLevel="0.5"/>
</effects>
```

### Effect Types

#### Filters

| Type | Attributes |
|------|------------|
| `lowpass` | frequency (0–22000), resonance (0.001–5.0) — 4-pole (24 dB/oct) |
| `lowpass_4pl` | frequency, resonance — legacy alias for `lowpass` (4-pole, 24 dB/oct) |
| `lowpass_1pl` | frequency (no resonance) — 1-pole (6 dB/oct) |
| `highpass` | frequency, resonance — 4-pole (24 dB/oct) |
| `bandpass` | frequency, resonance — 4-pole (24 dB/oct) |
| `notch` | frequency (60–22000), Q (0.01–18.0) |
| `peak` | frequency, Q, gain (0–1.0) |

#### Modulation

| Type | Attributes |
|------|------------|
| `chorus` | mix (0–1.0), modDepth (0–1.0), modRate (0–10.0 Hz) |
| `phaser` | mix, modDepth, modRate, centerFrequency, feedback (-1–1.0) |

#### Time-Based

| Type | Attributes |
|------|------------|
| `reverb` | roomSize (0–1.0), damping (0–1.0), wetLevel (0–1.0) |
| `delay` | delayTime (0–20.0), feedback (0–1.0), stereoOffset (-10–10), wetLevel, delayTimeFormat ("seconds"/"musical_time") |
| `convolution` | mix, irFile (required) |

#### Distortion

| Type | Attributes |
|------|------------|
| `wave_folder` | drive (1–100), threshold (0–10.0) |
| `wave_shaper` | drive (1–1000), driveBoost (0–1.0), outputLevel (0–1.0), highQuality (true/false) |
| `bit_crusher` | bitDepth (1–24), sampleRateReduction (1–32), mix (0–1.0) |

#### Other

| Type | Attributes |
|------|------------|
| `gain` | level (dB or linear with levelUnit), levelUnit ("decibels"/"linear") |
| `pitch_shift` | pitchShift (-24–24 semitones), mix (0–1.0) |
| `stereo_simulator` | algorithm ("lauridsen"/"schroeder"/"adt"), width (0–1), delayTime, modRate, modDepth |

All effects support a `tags` attribute for targeting by name instead of index.

---

## The `<modulators>` Element

Contains modulation sources: LFOs, envelopes, MIDI CC, velocity, MPE, random.

### `<lfo>` Element

| Attribute | Description |
|-----------|-------------|
| shape | "sine", "square", "saw" |
| frequency | Rate in Hz |
| modAmount | Depth (0.0–1.0, default: 1.0) |
| delayTime | Delay before start in seconds (default: 0.0) |
| scope | "global" (default) or "voice" |
| modBehavior | "add", "multiply", or "set" (default: "set") |

### `<envelope>` Element

| Attribute | Description |
|-----------|-------------|
| attack, decay, release | Times in seconds |
| sustain | Level (0.0–1.0) |
| modAmount | Depth (0.0–1.0) |
| scope | "global" or "voice" (default) |
| modBehavior | "add", "multiply", or "set" |
| attackCurve, decayCurve, releaseCurve | Curve shapes (-100 to 100) |

### `<midiCC>` Element

| Attribute | Description |
|-----------|-------------|
| number | MIDI CC number (0–127, required) |
| modAmount | Depth (0.0–1.0) |
| channel | "voice" (default), or specific channel 1–16 |
| scope | "global" or "voice" |

### `<midiVelocity>` Element

| Attribute | Description |
|-----------|-------------|
| modAmount | Depth (0.0–1.0) |
| scope | "voice" (default) or "global" |

### `<mpeTimbre>` / `<mpePressure>` Elements

| Attribute | Description |
|-----------|-------------|
| scope | "voice" (default) or "global" |
| risingSmoothingTime | Rise time in ms (default: 0) |
| fallingSmoothingTime | Fall time in ms (default: 0) |

### `<random>` Element

| Attribute | Description |
|-----------|-------------|
| mode | "note_on" or "periodic" |
| frequency | Rate for periodic mode |
| trigger | "attack" or "none" |
| seed | Integer seed for reproducibility |
| scope | "voice" or "global" |

---

## The `<buses>` Element

Up to 16 buses for audio routing.

```xml
<buses>
  <bus busVolume="1.0" output1Target="MAIN_OUTPUT" output1Volume="1.0">
    <effects>
      <effect type="reverb" wetLevel="1"/>
    </effects>
  </bus>
</buses>
```

| Attribute | Description |
|-----------|-------------|
| busVolume | Bus volume (0.0–1.0) |
| output1Target–output8Target | MAIN_OUTPUT, AUX_STEREO_OUTPUT_1–16 |
| output1Volume–output8Volume | Output volumes (0.0–1.0) |

Routes audio from groups via `output1Target="BUS_1"` etc.

---

## The `<midi>` Element

### `<cc>` — MIDI CC Mapping

```xml
<midi>
  <cc number="1">
    <binding level="ui" type="control" position="3" parameter="VALUE"
             translation="linear" translationOutputMin="0" translationOutputMax="1"/>
  </cc>
</midi>
```

### `<note>` — Note/Keyswitch Mapping

```xml
<midi>
  <note note="11" eventType="note_on" swallowNotes="false">
    <binding type="general" level="group" position="0" parameter="ENABLED"
             translation="fixed_value" translationValue="true"/>
  </note>
</midi>
```

| Attribute | Description |
|-----------|-------------|
| note | MIDI note number or range (e.g., "24-35") |
| eventType | "note_on", "note_off", or "any" |
| swallowNotes | If true, note is not passed to sampler |
| enabled | true/false |

### `<velocity>` — Velocity Mapping

```xml
<midi>
  <velocity>
    <binding type="effect" level="group" groupIndex="0" effectIndex="0"
             parameter="FX_FILTER_FREQUENCY" modAmount="0.3"/>
  </velocity>
</midi>
```

---

## The `<noteSequences>` Element

Built-in step sequencer.

```xml
<noteSequences>
  <sequence name="MajorArp" length="4" rate="1">
    <note position="0" velocity="1" note="C2" length="1"/>
    <note position="1" velocity="1" note="E2" length="1"/>
    <note position="2" velocity="1" note="G2" length="1"/>
    <note position="3" velocity="1" note="C3" length="1"/>
  </sequence>
</noteSequences>
```

`<sequence>` attributes: name, length (beats, required), rate (default: 1.0)
`<note>` attributes: position (beats), velocity (0–1), note (MIDI number or name), length (beats)

---

## The `<tags>` Element

Define tag properties for groups/samples.

```xml
<tags>
  <tag name="voice1" volume="1" pan="0" polyphony="12"/>
</tags>
```

| Attribute | Description |
|-----------|-------------|
| name | Tag identifier |
| enabled | true/false (default: true) |
| volume | 0.0–1.0 (default: 1.0) |
| polyphony | Voice limit (-1 = unlimited, 1 = monophonic) |

---

## The Binding System

Bindings connect sources (UI controls, MIDI, modulators) to targets (parameters).

### Binding Attributes

| Attribute | Required | Description |
|-----------|----------|-------------|
| type | Yes | "amp", "general", "effect", "control", "modulator", "note_sequence" |
| level | Yes | "ui", "instrument", "group", "tag", "bus" |
| position | Yes* | 0-based index of target element |
| parameter | Yes | Target parameter name |
| tags | No | Comma-separated tags (alternative to position) |
| groupTags | No | Target groups by tag |
| effectTags | No | Target effects by tag |
| controlTags | No | Target controls by tag |
| modulatorTags | No | Target modulators by tag |
| identifier | No | Tag identifier (for tag-level bindings) |
| translation | No | "linear" (default), "table", "fixed_value" |
| translationOutputMin | No | Min output for linear translation |
| translationOutputMax | No | Max output for linear translation |
| translationReversed | No | true/false |
| translationTable | No | Input/output pairs: "0,33;0.3,150;1.0,22000" |
| translationValue | No | Fixed value for fixed_value translation |
| enabled | No | true/false |
| triggerOnLoad | No | true/false (default: true) |

*position is required unless tags/identifier are used.

**triggerOnLoad:** Controls whether binding fires on preset load. Default: true. Set to false on VALUE bindings inside button states to prevent overwriting user's saved control positions when a DAW session is restored.

### Common Binding Parameters

**Amplitude (type="amp"):**

| Parameter | Level | Description |
|-----------|-------|-------------|
| AMP_VOLUME | instrument, group | Volume (0.0–16.0) |
| PAN | instrument, group | Pan (-100 to 100) |
| ENV_ATTACK | instrument, group | Attack time (0.0–10.0 seconds) |
| ENV_DECAY | instrument, group | Decay time (0.0–25.0 seconds) |
| ENV_SUSTAIN | instrument, group | Sustain level (0.0–1.0) |
| ENV_RELEASE | instrument, group | Release time (0.0–25.0 seconds) |
| ENV_ATTACK_CURVE | instrument, group | Attack curve (-100 to 100) |
| ENV_DECAY_CURVE | instrument, group | Decay curve (-100 to 100) |
| ENV_RELEASE_CURVE | instrument, group | Release curve (-100 to 100) |
| GLIDE_TIME | instrument, group | Portamento time (0.0–10.0 seconds) |
| GLIDE_MODE | instrument | "always", "legato", "off" |
| AMP_VEL_TRACK | instrument, group | Velocity sensitivity (0.0–1.0) |
| AMP_ENV_ENABLED | instrument, group | Enable amplitude envelope (true/false) |
| GROUP_VOLUME | group | Group volume (0.0–16.0) |
| GROUP_TUNING | group | Group pitch offset (-36.0 to 36.0 semitones) |
| GROUP_PAN | group | Group pan (-100 to 100) |
| OUTPUT_1_VOLUME–OUTPUT_8_VOLUME | group | Output routing volumes (0.0–1.0) |
| OUTPUT_1_TARGET–OUTPUT_8_TARGET | group | Output routing targets |
| ENABLED | group | Enable/disable group (true/false) |
| GLOBAL_TUNING | instrument | Global pitch offset (-36.0 to 36.0) |

**Effect (type="effect"):**

| Parameter | Description |
|-----------|-------------|
| FX_FILTER_FREQUENCY | Filter cutoff (0–22000 Hz) |
| FX_FILTER_RESONANCE | Filter resonance (0–5.0) |
| FX_FILTER_Q | Peak/notch Q (0.01–18.0) |
| FX_FILTER_GAIN | Peak filter gain (0–10.0) |
| FX_MIX | Wet/dry mix (0–1.0) |
| FX_REVERB_WET_LEVEL | Reverb wet level (0–1.0) |
| FX_REVERB_ROOM_SIZE | Reverb room size (0–1.0) |
| FX_REVERB_DAMPING | Reverb damping (0–1.0) |
| FX_DELAY_TIME | Delay time (0–1.0 or 0–20.0) |
| FX_DELAY_TIME_FORMAT | "seconds" or "musical_time" |
| FX_DELAY_FEEDBACK | Delay feedback (0–1.0) |
| FX_STEREO_OFFSET | Delay stereo offset (-10–10) |
| FX_WET_LEVEL | Delay wet level (0–1.0) |
| FX_CHORUS_MIX | Chorus mix (0–1.0) |
| FX_MOD_DEPTH | Chorus/phaser depth (0–1.0) |
| FX_MOD_RATE | Chorus/phaser rate (0–10.0 Hz) |
| FX_CENTER_FREQUENCY | Phaser center frequency (0–22000 Hz) |
| FX_FEEDBACK | Phaser feedback (-1–1.0) |
| FX_DRIVE | Wave folder/shaper drive (1–100 or 1–1000) |
| FX_THRESHOLD | Wave folder threshold (0–10.0) |
| FX_OUTPUT_LEVEL | Wave shaper output (0–8.0) |
| FX_DRIVE_BOOST | Wave shaper drive boost (0–1.0) |
| FX_PITCH_SHIFT | Pitch shift (-24–24 semitones) |
| FX_BIT_DEPTH | Bit crusher depth (1–24) |
| FX_SAMPLE_RATE_REDUCTION | Bit crusher reduction (1–32) |
| FX_WIDTH | Stereo simulator width (0–1) |
| FX_IR_FILE | Convolution IR file path |
| LEVEL | Gain level (0–8.0) |
| ENABLED | Enable/disable effect (true/false) |

**General (type="general"):**

| Parameter | Level | Description |
|-----------|-------|-------------|
| ENABLED | instrument, group, tag | Enable/disable element |
| SAMPLE_START | instrument, group | Sample start position (samples) |
| SAMPLE_END | instrument, group | Sample end position (samples) |
| LOOP_START | instrument, group | Loop start (samples) |
| LOOP_END | instrument, group | Loop end (samples) |
| LO_NOTE | instrument, group | Low note range (0–127) |
| HI_NOTE | instrument, group | High note range (0–127) |
| ROOT_NOTE | instrument, group | Root note (0–127) |
| PITCH_KEY_TRACK | instrument, group | Pitch tracking (0.0–1.0) |
| SILENCING_MODE | instrument, group | "fast" or "normal" |
| SILENCING_DECAY | instrument, group | Fade-out time (seconds) |
| OSCILLATOR_WAVEFORM | group | Waveform type |
| OSCILLATOR_WAVETABLE_POSITION | group | Wavetable position (0.0–1.0) |
| OSCILLATOR_WAVETABLE_FRAME_INTERPOLATION | group | Frame interpolation (true/false) |
| OSCILLATOR_DAMPING | group | Pluck damping (0.0–1.0) |
| OSCILLATOR_PLUCK_TYPE | group | Pluck type (0.0–1.0) |
| OSCILLATOR_HARMONIC_NUM_PARTIALS | group | Number of partials (1–64) |
| OSCILLATOR_HARMONIC_TILT | group | Spectral tilt (-1.0–1.0) |
| OSCILLATOR_HARMONIC_ODD_EVEN_BALANCE | group | Odd/even balance (0.0–1.0) |
| OSCILLATOR_HARMONIC_NORMALIZATION | group | Loudness compensation (0.0–1.0) |
| OSCILLATOR_HARMONIC_PARTIAL_1_LEVEL–64_LEVEL | group | Per-partial levels (0.0–1.0) |
| OSCILLATOR_FM_ALGORITHM | group | FM algorithm (1–32) |
| OSCILLATOR_FM_OP1_LEVEL–FM_OP6_LEVEL | group | FM operator levels (0.0–1.0) |
| OSCILLATOR_FM_OP1_RATIO–FM_OP6_RATIO | group | FM operator ratios |
| OSCILLATOR_FM_OP1_FEEDBACK–FM_OP6_FEEDBACK | group | FM feedback (0.0–1.0) |
| OSCILLATOR_FM_OP1_DETUNE–FM_OP6_DETUNE | group | FM detune (-7 to +7) |
| OSCILLATOR_FM_OP1_MODE–FM_OP6_MODE | group | "ratio" or "fixed" |
| OSCILLATOR_FM_OP1_FIXED_FREQ–FM_OP6_FIXED_FREQ | group | Fixed frequency (Hz) |
| OSCILLATOR_FM_OP1_VELOCITY_SENSITIVITY–FM_OP6_VELOCITY_SENSITIVITY | group | Velocity response (0–7) |
| OSCILLATOR_FM_OP1_ATTACK–FM_OP6_ATTACK | group | Per-operator attack (0–45 seconds) |
| OSCILLATOR_FM_OP1_DECAY–FM_OP6_DECAY | group | Per-operator decay (0–45 seconds) |
| OSCILLATOR_FM_OP1_SUSTAIN–FM_OP6_SUSTAIN | group | Per-operator sustain (0.0–1.0) |
| OSCILLATOR_FM_OP1_RELEASE–FM_OP6_RELEASE | group | Per-operator release (0–45 seconds) |
| OSCILLATOR_FM_OP1_EG_TYPE–FM_OP6_EG_TYPE | group | "adsr" or "dx7" |
| OSCILLATOR_FM_OP1_EG_RATE1–FM_OP6_EG_RATE4 | group | DX7 rates (0–99) |
| OSCILLATOR_FM_OP1_EG_LEVEL1–FM_OP6_EG_LEVEL4 | group | DX7 levels (0–99) |
| ALL_NOTES_OFF | instrument | Stop all notes (true) |

**Control (type="control", level="ui"):**

| Parameter | Description |
|-----------|-------------|
| VALUE | Set control value |
| TEXT | Set label text |
| VISIBLE | Show/hide control (true/false) |
| ENABLED | Enable/disable control (true/false) |
| MIN_VALUE | Set minimum value |
| MAX_VALUE | Set maximum value |
| VALUE_TYPE | "float", "integer", "musical_time" |
| PATH | Image/animation file path |
| OPACITY | Image/animation opacity (0.0–1.0) |
| FRAME_RATE | Animation frame rate (0–24) |
| CURRENT_FRAME | Animation current frame |
| PLAYBACK_MODE | "forward_loop", "forward_once", "reverse_loop", "reverse_once", "ping_pong_loop", "stopped" |
| X_VALUE, Y_VALUE | XY pad values (0.0–1.0) |
| X, Y | Rectangle/line position |
| WIDTH, HEIGHT | Rectangle dimensions |
| X1, Y1, X2, Y2 | Line endpoints |

**Keyboard Color (type="keyboard_color", level="ui"):**

| Parameter | Description |
|-----------|-------------|
| ENABLED | Show/hide color (true/false) |
| LO_NOTE | Low note range (0–127) |
| HI_NOTE | High note range (0–127) |

Requires `colorIndex` attribute (0-based index of color element in `<keyboard>`).

**Tag (type="general", level="tag"):**

| Parameter | Description |
|-----------|-------------|
| TAG_ENABLED | Enable/disable tag (true/false) |
| TAG_VOLUME | Tag volume (0.0–16.0) |
| TAG_POLYPHONY | Voice limit (-1 to 127) |

Requires `identifier` attribute with the tag name.

**Modulator (type="modulator", level="instrument"):**

| Parameter | Description |
|-----------|-------------|
| MOD_AMOUNT | Modulation depth (0.0–1.0) |
| FREQUENCY | LFO rate (0.0–22000.0 Hz) |
| SHAPE | LFO shape ("sine", "square", "saw") |
| MOD_DELAY_TIME | Delay before start (0.0–10000.0 seconds) |
| TRIGGER | Reset behavior ("attack" or "none") |
| ENV_ATTACK | Envelope attack (0.0–10.0 seconds) |
| ENV_DECAY | Envelope decay (0.0–25.0 seconds) |
| ENV_SUSTAIN | Envelope sustain (0.0–1.0) |
| ENV_RELEASE | Envelope release (0.0–25.0 seconds) |
| ENV_ATTACK_CURVE | Attack curve (-100 to 100) |
| ENV_DECAY_CURVE | Decay curve (-100 to 100) |
| ENV_RELEASE_CURVE | Release curve (-100 to 100) |

**Note Sequence (type="note_sequence", level="instrument"):**

| Parameter | Description |
|-----------|-------------|
| RATE | Sequence rate (0.01–100) |

**Note Sequence Binding Attributes:**

| Attribute | Description |
|-----------|-------------|
| seqIndex | 0-based index of sequence (required) |
| seqLoopMode | "forward", "reverse", "random", "random_no_repeat", "no_loop" |
| seqTriggerBehavior | "midi_key", "on", "off" |
| seqPlayerIdentifier | String identifier for tracking sequence state |
| seqTransposeWithRootNote | Transpose relative to incoming MIDI note (0–127) |
| seqTrackMidiInputVelocity | Respect incoming velocity (0.0–1.0) |
| seqPlaybackRate | Playback speed (0.001–10000) |
| seqTranspose | Transpose in semitones (-36 to 36) |
| seqFollowGlobalTempo | Follow DAW tempo (true/false) |

**MIDI Binding Parameters:**

| Parameter | Level | Description |
|-----------|-------|-------------|
| ENABLED | midi | Enable/disable binding |
| SEQ_INDEX | note_binding | Change sequence index |
| SEQ_LOOP_MODE | note_binding | Change loop mode |
| SEQ_TRANSPOSE_WITH_ROOT_NOTE | note_binding | Change transpose setting |
| SEQ_PLAYBACK_RATE | note_binding | Change playback rate |
| SEQ_TRACK_MIDI_INPUT_VELOCITY | note_binding | Change velocity tracking |
| SEQ_TRANSPOSE | note_binding | Change transpose amount |

**Bus (type="amp", level="bus"):**

| Parameter | Description |
|-----------|-------------|
| BUS_VOLUME | Bus volume (0.0–16.0) |
| OUTPUT_1_VOLUME–OUTPUT_8_VOLUME | Output volumes (0.0–1.0) |
| OUTPUT_1_TARGET–OUTPUT_8_TARGET | Output routing targets |

### Translation Modes

**linear:** Maps input range to output range.
```xml
translation="linear" translationOutputMin="0" translationOutputMax="22000"
```

**table:** Non-linear mapping with coordinate pairs.
```xml
translation="table" translationTable="0,33;0.3,150;0.5,1100;0.9,11000;1.0001,22000"
```
Format: `input,output;input,output;...`

**fixed_value:** Sets a constant value.
```xml
translation="fixed_value" translationValue="true"
```

---

## Boilerplate Template

```xml
<?xml version="1.0" encoding="UTF-8"?>

<DecentSampler minVersion="1.0.0">
  <ui width="812" height="375" bgColor="FF1A1A2E">
    <tab name="main">
      <!-- Position 0 in UI controls (0-indexed) -->
      <labeled-knob x="445" y="75" width="90" textSize="16" textColor="FF000000"
                    trackForegroundColor="CC000000" trackBackgroundColor="66999999"
                    label="Attack" type="float" minValue="0.0" maxValue="4.0" value="0.01">
        <binding type="amp" level="instrument" position="0" parameter="ENV_ATTACK"/>
      </labeled-knob>
      <!-- Position 1 in UI controls -->
      <labeled-knob x="515" y="75" width="90" textSize="16" textColor="FF000000"
                    trackForegroundColor="CC000000" trackBackgroundColor="66999999"
                    label="Release" type="float" minValue="0.0" maxValue="20.0" value="1">
        <binding type="amp" level="instrument" position="0" parameter="ENV_RELEASE"/>
      </labeled-knob>
      <!-- Position 2 in UI controls. Effect position 0 = lowpass filter -->
      <labeled-knob x="585" y="75" width="90" textSize="16" textColor="FF000000"
                    trackForegroundColor="CC000000" trackBackgroundColor="66999999"
                    label="Tone" type="float" minValue="0" maxValue="1" value="1">
        <binding type="effect" level="instrument" position="0" parameter="FX_FILTER_FREQUENCY"
                 translation="table"
                 translationTable="0,33;0.3,150;0.4,450;0.5,1100;0.7,4100;0.9,11000;1.0001,22000"/>
      </labeled-knob>
      <!-- Position 3 in UI controls. Effect position 1 = reverb -->
      <labeled-knob x="655" y="75" width="90" textSize="16" textColor="FF000000"
                    trackForegroundColor="CC000000" trackBackgroundColor="66999999"
                    label="Reverb" type="percent" minValue="0" maxValue="100" value="50">
        <binding type="effect" level="instrument" position="1"
                 parameter="FX_REVERB_WET_LEVEL" translation="linear"
                 translationOutputMin="0" translationOutputMax="1"/>
      </labeled-knob>
    </tab>
    <keyboard/>
  </ui>

  <effects>
    <!-- Effect position 0 -->
    <effect type="lowpass" frequency="22000.0"/>
    <!-- Effect position 1 -->
    <effect type="reverb" wetLevel="0.5"/>
  </effects>

  <groups attack="0.0" decay="1.0" sustain="0.0" release="1.0" ampVelTrack="0.3">
    <!-- Group position 0 -->
    <group name="Main">
      <sample path="Samples/YourSample.wav" rootNote="60" loNote="0" hiNote="127"/>
    </group>
  </groups>

  <midi>
    <cc number="1">
      <!-- CC1 (Mod Wheel) controls the Tone knob (UI position 2) -->
      <binding level="ui" type="control" position="2" parameter="VALUE"
               translation="linear" translationOutputMin="0" translationOutputMax="1"/>
    </cc>
  </midi>
</DecentSampler>
```

---

## Common Patterns

### Keyswitches

```xml
<midi>
  <note note="11" eventType="note_on">
    <binding type="general" level="group" position="0" parameter="ENABLED"
             translation="fixed_value" translationValue="true"/>
    <binding type="general" level="group" position="1" parameter="ENABLED"
             translation="fixed_value" translationValue="false"/>
  </note>
  <note note="12" eventType="note_on">
    <binding type="general" level="group" position="0" parameter="ENABLED"
             translation="fixed_value" translationValue="false"/>
    <binding type="general" level="group" position="1" parameter="ENABLED"
             translation="fixed_value" translationValue="true"/>
  </note>
</midi>
```

### MPE Support

```xml
<modulators>
  <mpeTimbre scope="voice" fallingSmoothingTime="20" risingSmoothingTime="20">
    <binding type="effect" level="group" groupIndex="0" effectIndex="0"
             parameter="FX_FILTER_FREQUENCY" modBehavior="add"
             translation="table"
             translationTable="0,33;0.3,150;0.5,1100;0.9,11000;1.0001,22000"/>
  </mpeTimbre>
  <mpePressure scope="voice" fallingSmoothingTime="20" risingSmoothingTime="20">
    <binding type="amp" level="group" groupIndex="0" parameter="AMP_VOLUME"
             modBehavior="set" translation="linear"
             translationOutputMin="0.5" translationOutputMax="1"/>
  </mpePressure>
</modulators>
```

### Tempo-Synced Delay

```xml
<ui>
  <tab>
    <labeled-knob x="180" y="40" label="Delay Time" valueType="musical_time"
                  minValue="0" maxValue="20" value="10">
      <binding type="effect" level="instrument" position="0" parameter="FX_DELAY_TIME"/>
    </labeled-knob>
  </tab>
</ui>
<effects>
  <effect type="delay" delayTimeFormat="musical_time" delayTime="10"
          stereoOffset="0.01" feedback="0.3" wetLevel="0.5"/>
</effects>
```

### Reverb Send Bus

```xml
<buses>
  <bus busVolume="1.0" output1Target="MAIN_OUTPUT" output1Volume="1.0">
    <effects>
      <effect type="reverb" wetLevel="1"/>
    </effects>
  </bus>
</buses>
<groups>
  <group output1Target="MAIN_OUTPUT" output1Volume="0.0"
         output2Target="BUS_1" output2Volume="0.2">
    <sample path="Samples/Piano.wav" rootNote="60" loNote="0" hiNote="127"/>
  </group>
</groups>
```

### Wavetable Scanning

```xml
<groups>
  <group tags="wt" wavetablePosition="0.5">
    <oscillator waveform="wavetable" wavetableFile="Wavetables/MyWavetable.wav"/>
  </group>
</groups>
<ui>
  <tab>
    <labeled-knob x="10" y="20" width="90" label="Position" type="float"
                  minValue="0" maxValue="1" value="0.5">
      <binding type="general" level="group" tags="wt"
               parameter="OSCILLATOR_WAVETABLE_POSITION"
               translation="linear" translationOutputMin="0.0" translationOutputMax="1.0"/>
    </labeled-knob>
  </tab>
</ui>
```

### FM6 Synthesis

```xml
<groups>
  <group fmAlgorithm="5"
         fmOp1Ratio="1.0" fmOp1Level="1.0"
         fmOp2Ratio="2.0" fmOp2Level="0.7"
         fmOp3Ratio="1.0" fmOp3Level="1.0"
         fmOp4Ratio="7.0" fmOp4Level="0.5"
         fmOp5Ratio="1.0" fmOp5Level="1.0"
         fmOp6Ratio="14.0" fmOp6Level="0.4">
    <oscillator waveform="fm6op"/>
  </group>
</groups>
```

### Monophonic Lead with Portamento

```xml
<tags>
  <tag name="osc1" polyphony="1"/>
</tags>
<groups glideTime="0.3" glideMode="legato">
  <group tags="osc1">
    <oscillator waveform="saw"/>
  </group>
</groups>
```

### True Legato (3-Group Pattern)

```xml
<groups>
  <!-- Initial sustain: plays on first note -->
  <group trigger="first" tags="sustain" silencedByTags="legato"
         silencingMode="normal" release="0.3">
    <sample path="Samples/Sustain.wav" rootNote="60" loNote="0" hiNote="127"/>
  </group>
  
  <!-- Legato transitions: plays when notes overlap -->
  <group trigger="legato" tags="legato" silencedByTags="legato"
         silencingMode="normal" attack="0.1">
    <sample path="Samples/Legato_C3_D3.wav" rootNote="62" loNote="62" hiNote="62"
            start="43000" previousNotes="60"/>
    <sample path="Samples/Legato_D3_E3.wav" rootNote="64" loNote="64" hiNote="64"
            start="43000" previousNotes="62"/>
  </group>
  
  <!-- Optional: second sustain after legato -->
  <group trigger="legato" tags="legato-tails" silencedByTags="legato-tails"
         attack="0.3" silencingMode="normal" release="0.3">
    <sample path="Samples/Sustain.wav" rootNote="60" loNote="0" hiNote="127"/>
  </group>
</groups>

<tags>
  <tag name="sustain" polyphony="1"/>
  <tag name="legato" polyphony="1"/>
  <tag name="legato-tails" polyphony="1"/>
</tags>
```

### Drum Voice Muting

```xml
<groups>
  <group tags="hihat" silencedByTags="hihat" silencingMode="fast">
    <sample path="Samples/HiHat.wav" rootNote="60" loNote="60" hiNote="60"/>
  </group>
  <group tags="hihat-open" silencedByTags="hihat" silencingDecay="0.02">
    <sample path="Samples/HiHatOpen.wav" rootNote="61" loNote="61" hiNote="61"/>
  </group>
</groups>
```

### Tags Control (Mic-Level Knobs)

```xml
<tags>
  <tag name="mic1" volume="1"/>
  <tag name="mic2" volume="1"/>
</tags>
<groups>
  <group tags="note">
    <sample tags="mic1" path="Samples/Mic1.wav" rootNote="60" loNote="0" hiNote="127"/>
    <sample tags="mic2" path="Samples/Mic2.wav" rootNote="60" loNote="0" hiNote="127"/>
  </group>
</groups>
<ui>
  <tab>
    <control x="246" y="115" parameterName="MIC 1" style="linear_bar_vertical"
             type="float" minValue="0" maxValue="100" value="60" width="20" height="70">
      <binding type="amp" level="tag" identifier="mic1" parameter="AMP_VOLUME"/>
    </control>
    <control x="346" y="115" parameterName="MIC 2" style="linear_bar_vertical"
             type="float" minValue="0" maxValue="100" value="60" width="20" height="70">
      <binding type="amp" level="tag" identifier="mic2" parameter="AMP_VOLUME"/>
    </control>
  </tab>
</ui>
```

### Waveform Switching via Menu

```xml
<ui>
  <tab>
    <menu x="10" y="70" width="150" height="30" value="1">
      <option name="Sine">
        <binding type="general" level="group" position="0"
                 parameter="OSCILLATOR_WAVEFORM"
                 translation="fixed_value" translationValue="sine"/>
      </option>
      <option name="Saw">
        <binding type="general" level="group" position="0"
                 parameter="OSCILLATOR_WAVEFORM"
                 translation="fixed_value" translationValue="saw"/>
      </option>
      <option name="Square">
        <binding type="general" level="group" position="0"
                 parameter="OSCILLATOR_WAVEFORM"
                 translation="fixed_value" translationValue="square"/>
      </option>
    </menu>
  </tab>
</ui>
<groups>
  <group>
    <oscillator waveform="sine"/>
  </group>
</groups>
```

### Wavetable Selector (TAG_ENABLED Pattern)

```xml
<ui>
  <tab>
    <menu x="20" y="68" width="220" height="26" value="1">
      <option name="Growl 01">
        <binding type="amp" level="tag" identifier="growl" parameter="TAG_ENABLED"
                 translation="fixed_value" translationValue="true"/>
        <binding type="amp" level="tag" identifier="fm" parameter="TAG_ENABLED"
                 translation="fixed_value" translationValue="false"/>
      </option>
      <option name="FM 01">
        <binding type="amp" level="tag" identifier="growl" parameter="TAG_ENABLED"
                 translation="fixed_value" translationValue="false"/>
        <binding type="amp" level="tag" identifier="fm" parameter="TAG_ENABLED"
                 translation="fixed_value" translationValue="true"/>
      </option>
    </menu>
  </tab>
</ui>
<groups>
  <group tags="growl">
    <oscillator waveform="wavetable" wavetableFile="Wavetables/Growl.wav" randomPhase="true"/>
  </group>
  <group tags="fm" enabled="false">
    <oscillator waveform="wavetable" wavetableFile="Wavetables/FM.wav" randomPhase="true"/>
  </group>
</groups>
```

### CC-Triggered Samples (Piano Pedal)

```xml
<groups>
  <group>
    <sample path="Samples/PedalUp.wav" rootNote="60" loNote="60" hiNote="60"
            onLoCC64="0" onHiCC64="63"/>
    <sample path="Samples/PedalDown.wav" rootNote="60" loNote="60" hiNote="60"
            onLoCC64="90" onHiCC64="127"/>
  </group>
</groups>
```

---

## Validation & Cleanup Rules

1. **XML declaration required:** Always start with `<?xml version="1.0" encoding="UTF-8"?>`
2. **Path case sensitivity:** File paths are case-sensitive on Linux. Validate with DecentSampler's Developer Tools > Validate Preset.
3. **Group-level effects:** Only lowpass, highpass, bandpass, gain, chorus work at group level. Delay and reverb will be deleted before their tail ends.
4. **minVersion for oscillators:** Use minVersion="1.15.0" or higher for oscillator features. Use 1.19.0 for wavetable features.
5. **Menu indices are 1-based:** `value="1"` selects the first option.
6. **Binding positions are 0-based:** First group = position 0, first effect = position 0.
7. **Effect tags over indices:** Prefer `tags` and `effectTags` over numeric indices for resilience against reorderings.
8. **Empty elements:** `<effects/>`, `<midi/>`, `<tags/>`, `<buses/>`, `<modulators/>`, `<noteSequences/>` are valid when empty.
9. **Sample paths relative:** All paths are relative to the .dspreset file location.
10. **Verify referenced files:** Ensure all sample, image, wavetable, and IR files exist at the specified paths.
11. **`lowpass_4pl` is legacy:** Use `lowpass` instead of `lowpass_4pl` for new presets; `lowpass_4pl` is a legacy alias and functionally identical.

---

## Examples Directory Reference

The `DecentSampler-Sample-Library-Examples/` directory contains working examples for every feature:

| Directory | Feature |
|-----------|---------|
| example-001-boilerplate | Minimal starting preset |
| example-002-how-to-make-buttons | Text and image buttons |
| example-003-how-to-use-convolution-reverb | Convolution IR effects |
| example-004-wave-shaper-and-wave-folder | Distortion effects |
| example-005-how-to-code-tempo-synced-delay | Musical time delay |
| example-006-working-with-note-sequences | Step sequencer |
| example-007-how-to-code-lfos-and-envelopes | All modulator types |
| example-008-adjusting-sample-loop-parameters-with-knobs | Loop control |
| example-009-how-to-use-the-xy-pad-ui-control | XY pad control |
| example-010-how-to-use-audio-buses-and-external-outputs | Bus routing |
| example-011-how-to-use-animations | Multi-frame animations |
| example-012-mpe | MPE timbre/pressure |
| example-013-controlling-velocity-sensitivity | Velocity and polyphony |
| example-014-effects | Pitch shift and other effects |
| example-015-how-to-using-continuous-background-sounds | Looping background audio |
| example-016-drawing-with-lines-and-rectangles | UI drawing primitives |
| example-017-oscillators | Wavetable, FM6, pluck, harmonic |
