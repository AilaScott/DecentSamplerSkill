# DecentSampler Skills

AI-assisted creation, editing, and validation of `.dspreset` files for [DecentSampler](https://www.decentsampler.com/), a free, open-source sample instrument player by Dave Hilowitz.

## What's Inside

- **SKILL.md** — Complete format reference covering XML structure, UI controls, effects, oscillators (wavetable, FM6, pluck, harmonic), modulators, buses, note sequences, keyswitches, MPE, bindings, and validation rules.

## Quick Start

### 1. Create a Minimal Preset

Save this as `My Instrument.dspreset`:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<DecentSampler minVersion="1.0.0">
  <ui width="812" height="375" bgColor="FF1A1A2E">
    <tab name="main">
      <labeled-knob x="445" y="75" width="90" textSize="16" textColor="FF000000"
                    trackForegroundColor="CC000000" trackBackgroundColor="66999999"
                    label="Attack" type="float" minValue="0.0" maxValue="4.0" value="0.01">
        <binding type="amp" level="instrument" position="0" parameter="ENV_ATTACK"/>
      </labeled-knob>
      <labeled-knob x="515" y="75" width="90" textSize="16" textColor="FF000000"
                    trackForegroundColor="CC000000" trackBackgroundColor="66999999"
                    label="Release" type="float" minValue="0.0" maxValue="20.0" value="1">
        <binding type="amp" level="instrument" position="0" parameter="ENV_RELEASE"/>
      </labeled-knob>
    </tab>
  </ui>

  <effects>
    <effect type="lowpass" frequency="22000.0"/>
    <effect type="reverb" wetLevel="0.5"/>
  </effects>

  <groups attack="0.0" decay="1.0" sustain="0.0" release="1.0">
    <group name="Main">
      <sample path="Samples/YourSample.wav" rootNote="60" loNote="0" hiNote="127"/>
    </group>
  </groups>
</DecentSampler>
```

### 2. Add Your Samples

Create a `Samples/` folder next to your `.dspreset` file and place your audio files there.

### 3. Open in DecentSampler

Load the `.dspreset` file in DecentSampler (free VST/AU plugin or standalone app).

---

## File Structure

A DecentSampler instrument folder:

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

All paths in the .dspreset file are relative to its location.

---

## Format Reference

DecentSampler presets are XML files with this hierarchy:

```
<DecentSampler>
  ├── <tags>              Tag definitions (polyphony, volume)
  ├── <ui>                User interface (812×375 recommended)
  │   └── <tab>           Single tab with controls
  │       ├── <labeled-knob>   Rotary dials
  │       ├── <menu>           Dropdown menus
  │       ├── <button>         Text/image buttons
  │       ├── <label>          Static text
  │       ├── <image>          Static images
  │       ├── <rectangle>      Filled rectangles
  │       ├── <line>           Line segments
  │       ├── <xyPad>          2D pad control
  │       ├── <oscilloscope>   Live waveform display
  │       └── <multiFrameImage> Animations
  ├── <groups>            Sample/oscillator groups
  │   └── <group>         Individual groups
  │       ├── <sample>    Audio samples
  │       ├── <oscillator> Synthetic waveforms
  │       └── <effects>   Group-level effects
  ├── <effects>           Global effects chain
  ├── <modulators>        LFOs, envelopes, MIDI CC, MPE, random
  ├── <buses>             Audio routing buses
  ├── <midi>              CC, note, velocity mappings
  └── <noteSequences>     Step sequencer
```

See `SKILL.md` for the complete attribute reference.

---

## Integration with AI Coding Tools

### Claude (Anthropic)

**Option 1: CLAUDE.md (project-level)**

Copy the contents of `SKILL.md` into a `CLAUDE.md` file in your project root:

```bash
cp SKILL.md /path/to/your-project/CLAUDE.md
```

Claude Code and claude.ai will automatically read `CLAUDE.md` files.

**Option 2: System Prompt (API)**

Paste the SKILL.md contents into your system prompt when using the Claude API.

**Option 3: Custom Instructions (claude.ai)**

Paste a condensed version of SKILL.md into your Custom Instructions in Claude.ai settings.

### Cursor

**Option 1: .cursorrules (project-level)**

Copy SKILL.md contents into `.cursorrules` in your project root:

```bash
cp SKILL.md /path/to/your-project/.cursorrules
```

Cursor automatically reads this file and applies it as context.

**Option 2: Project Rules (Cursor Settings)**

1. Open Cursor Settings (gear icon)
2. Go to Project Settings > Rules
3. Paste the SKILL.md contents

**Option 3: .cursor/rules/ directory**

For modular rules, create `.cursor/rules/` and add SKILL.md there. Cursor scans this directory.

### Codex (OpenAI)

**Option 1: AGENTS.md (project-level)**

Copy SKILL.md into `AGENTS.md` in your project root:

```bash
cp SKILL.md /path/to/your-project/AGENTS.md
```

Codex CLI reads `AGENTS.md` for project context.

**Option 2: CODEX.md**

Some Codex implementations also read `CODEX.md`. Copy there if your setup supports it:

```bash
cp SKILL.md /path/to/your-project/CODEX.md
```

**Option 3: System Prompt (API)**

When using the OpenAI API directly, paste SKILL.md contents into your system message.

### opencode

**Option 1: Skill (recommended)**

Place the files in the opencode skill directory:

```bash
mkdir -p ~/.config/opencode/skills/decent-sampler/
cp SKILL.md ~/.config/opencode/skills/decent-sampler/SKILL.md
```

opencode automatically loads skills from `~/.config/opencode/skills/` and scans for `**/SKILL.md` files.

**Option 2: Project-level skill**

For project-specific use:

```bash
mkdir -p .opencode/skills/decent-sampler/
cp SKILL.md .opencode/skills/decent-sampler/SKILL.md
```

**Option 3: AGENTS.md**

Copy SKILL.md to `AGENTS.md` in your project root for agent context:

```bash
cp SKILL.md /path/to/your-project/AGENTS.md
```

**Option 4: opencode.json**

Reference the skill path in your config:

```json
{
  "skills": {
    "paths": [".opencode/skills"]
  }
}
```

---

## Examples Directory

The `DecentSampler-Sample-Library-Examples/` directory contains 40+ working examples covering every feature:

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

Browse the examples for working code you can reference or adapt.

---

## Common Tasks

### Create a Preset with Effects

```xml
<effects>
  <effect type="lowpass" frequency="8000"/>
  <effect type="chorus" mix="0.3" modDepth="0.2" modRate="0.5"/>
  <effect type="reverb" wetLevel="0.4" roomSize="0.7"/>
</effects>
```

### Add Keyswitches

```xml
<midi>
  <note note="11" eventType="note_on">
    <binding type="general" level="group" position="0" parameter="ENABLED"
             translation="fixed_value" translationValue="true"/>
    <binding type="general" level="group" position="1" parameter="ENABLED"
             translation="fixed_value" translationValue="false"/>
  </note>
</midi>
```

### Add LFO Vibrato

```xml
<modulators>
  <lfo shape="sine" frequency="5.0" modAmount="0.15" scope="voice">
    <binding type="amp" level="group" position="0" parameter="GROUP_TUNING"
             modBehavior="add" translation="linear"
             translationOutputMin="-0.5" translationOutputMax="0.5"/>
  </lfo>
</modulators>
```

### Create a Wavetable Preset

```xml
<groups>
  <group tags="wt" wavetablePosition="0.5">
    <oscillator waveform="wavetable" wavetableFile="Wavetables/MyTable.wav"/>
  </group>
</groups>
```

---

## Validation

Use DecentSampler's built-in validation tool:

1. Open your `.dspreset` in DecentSampler
2. Go to **File > Developer Tools > Validate Preset**
3. Fix any reported errors

**Common issues:**
- Missing file references (check paths are correct and case-sensitive)
- Unclosed XML tags
- Incorrect attribute values
- Wrong minVersion for features used

---

## Testing Workflow

1. Create or edit the `.dspreset` file
2. Open DecentSampler (VST/AU plugin or standalone)
3. Load your preset: **File > Open Preset**
4. Play notes on MIDI keyboard or on-screen keyboard
5. If issues, validate: **File > Developer Tools > Validate Preset**
6. Fix errors and repeat from step 3

---

## Troubleshooting

| Error | Cause | Fix |
|-------|-------|-----|
| "Missing file reference" | Sample/image path doesn't exist | Check path spelling and case sensitivity |
| "Incorrect path capitalization" | Path matches case-insensitively but not exactly | Match the exact case on disk |
| No sound | No samples in groups, or note range doesn't match played notes | Check loNote/hiNote values |
| Group-level reverb not working | Reverb/delay at group level | Only lowpass, highpass, bandpass, gain, chorus work at group level |
| Wavetable not loading | Wrong minVersion | Set minVersion="1.15.0" or higher |
| FM6 parameters ignored | Attributes on oscillator instead of group | FM6 attributes go on `<group>`, not `<oscillator>` |
| Menu doesn't respond | Wrong index base | Menu indices are 1-based, not 0-based |
| Binding doesn't fire | Wrong position index | Binding positions are 0-based |
| Controls don't update from MIDI | MIDI CC targets parameter directly | Use `level="ui" type="control" parameter="VALUE"` to update the UI control |
| Legato plays wrong samples | previousNotes mismatch | Check that previousNotes values match the previous group's rootNote values |

---

## Choosing an AI Tool

| Tool | Best For | Integration Method |
|------|----------|-------------------|
| Claude Code | Full IDE integration, complex edits | CLAUDE.md file |
| Claude.ai | Quick questions, format lookup | Custom Instructions |
| Cursor | IDE with AI autocomplete | .cursorrules file |
| Codex CLI | Terminal-based coding | AGENTS.md file |
| opencode | Configurable agent system | SKILL.md in skills directory |

For all tools, the SKILL.md content serves as the knowledge base. Copy it to the appropriate location for your tool.

---

## Resources

- [DecentSampler Developer Guide](https://decentsampler-developers-guide.readthedocs.io/)
- [DecentSampler Plugin](https://www.decentsampler.com/)
- [DecentSamples Blog](https://www.decentsamples.com/blog/)
- [KnobMan Gallery](https://www.g200kg.com/en/webknobman/gallery.php) (custom knob skins)

---

## License

These skill files are provided as-is for use with DecentSampler preset development. See `DecentSampler-Sample-Library-Examples/LICENSE` for the examples license.
