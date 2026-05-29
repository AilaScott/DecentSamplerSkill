Most agents have basically no knowledge of DecentSampler, so I made a comprehensive agent skill for developing, troubleshooting, and fixing libraries for DecentSampler!

Tested with OpenCode and Deepseek V4 Flash.

**Sourced from:** [DecentSampler-Sample-Library-Examples](https://github.com/DBraun/DecentSampler-Sample-Library-Examples) (40+ working examples) and [official DecentSampler documentation](https://decentsampler-developers-guide.readthedocs.io/en/stable/index.html) for version **1.22.2**.

---

## What's Covered

| Category | Topics |
|----------|--------|
| **File Format** | XML structure, root element, minVersion strategy, ARGB color format |
| **UI System** | Tab, labeled-knob/control, menu, button, label, image, rectangle, line, xyPad, multiFrameImage (animation), oscilloscope, keyboard colors |
| **Effects** | Filters (lowpass, highpass, bandpass, notch, peak), chorus, phaser, reverb, delay, convolution, wave folder/shaper, bit crusher, gain, pitch shift, stereo simulator |
| **Oscillators** | Wavetable, FM6 (6-op DX7-style), pluck, harmonic/additive |
| **Modulation** | LFO, envelope (ADSR, DX7), MIDI CC, velocity, MPE timbre/pressure, random |
| **Routing** | Groups, samples, buses (1–16), output routing (MAIN_OUTPUT, AUX_STEREO_OUTPUT) |
| **MIDI** | CC mappings, note/keyswitch mappings, velocity mappings |
| **Sequencer** | Built-in note sequences with loop modes, playback controls |
| **Bindings** | Full binding system: amp, general, effect, control, modulator, note_sequence, keyboard_color, tag — with linear, table, and fixed_value translation |
| **Tags** | Tag-based polyphony control, volume, enable/disable |
| **Patterns** | Keyswitches, MPE, tempo-synced delay, reverb send bus, wavetable scanning, FM6, monophonic lead, true legato, drum voice muting, mic-level knobs, waveform switching, CC-triggered samples |
| **Validation** | 10 cleanup rules, common errors, testing workflow, troubleshooting table |

The full reference is in `SKILL.md` (~1340 lines).

---

## Usage

### Opencode

Create a subdirectory called `decentsampler` in `~/.config/opencode/skills/` and copy `SKILL.md` into it:

```bash
mkdir -p ~/.config/opencode/skills/decentsampler/
cp SKILL.md ~/.config/opencode/skills/decentsampler/
```

Start opencode and run `/skills` to select it.

### Claude Code

Copy to your project root as `CLAUDE.md`:

```bash
cp SKILL.md /path/to/your-project/CLAUDE.md
```

Claude Code automatically reads `CLAUDE.md` for project context. All `.dspreset` questions will have the full format reference available.

### Codex CLI

Copy to your project root as `AGENTS.md`:

```bash
cp SKILL.md /path/to/your-project/AGENTS.md
```

Codex CLI reads `AGENTS.md` for project-level context when working on DecentSampler instruments.

---



---

## Resources

- [DecentSampler Developer Guide](https://decentsampler-developers-guide.readthedocs.io/)
- [DecentSampler Plugin](https://www.decentsampler.com/)
- [DecentSampler-Sample-Library-Examples](https://github.com/DBraun/DecentSampler-Sample-Library-Examples)
