# MAGI-2 Open-Source 114B vs 480B MoE & Prompt Cheat Sheet

Start here: [Magi2Pro](https://magi2pro.com/) | [MAGI-2 Open Source](https://magi2pro.com/magi-2-open-source) | [MAGI-2 Prompts](https://magi2pro.com/magi-2-prompts) | [MAGI-2 vs Kling 3.0](https://magi2pro.com/magi-2-vs-kling-3-0) | [MAGI-2 vs Seedance 2.5](https://magi2pro.com/magi-2-vs-seedance-2-5)

## Model Specs

| Feature | Open-Source 114B Preview (Apache-2.0) | Hosted 480B MoE (Magi2Pro) |
|---|---|---|
| Total Parameters | 114B | 480B (Mixture of Experts) |
| Active Parameters per Forward | 114B | ~28B |
| Max Resolution | 1080p | 1080p 25fps |
| Native Audio | No | Yes (single-pass, synced dialogue + lip-sync + ambient SFX) |
| Max Clip Length | 4s | 4–30s continuous multi-shot |
| Omni-Reference Inputs | 0 | Up to 20 (characters, scenes, props, style, audio) |
| Pricing | Free weights | Transparent 8 credits/sec, failover routing |
| License | Apache-2.0 | Proprietary hosted |

## 5-Part Prompt Structure

Copy-paste this formula for best results:

```
[SCENE] [SUBJECT] [ACTION] [STYLE/ATMOSPHERE] [AUDIO]
```

**Example:**
```
[SCENE: neon-lit cyberpunk alley at midnight, rain reflecting on wet pavement]
[SUBJECT: a silver-haired android with glowing blue eyes, leather jacket]
[ACTION: walks slowly toward camera, turns head, smirks]
[STYLE: cinematic, anamorphic flares, grain, Blade Runner aesthetic]
[AUDIO: heavy rain, distant thunder, footsteps on metal grate, low synth drone]
```

### Tips per part
- **SCENE**: 1–3 specific visual anchors (lighting, location, time of day)
- **SUBJECT**: Physical description + key prop/outfit
- **ACTION**: Single continuous motion (avoid cuts)
- **STYLE**: 2–3 technical descriptors (e.g., "35mm lens, shallow DOF, teal-orange grade")
- **AUDIO**: 3–5 distinct sound layers (dialogue must be wrapped in `"..."`)

## Native Dialogue Audio Formatting

For synced speech in hosted 480B:

```
[AUDIO: "I told you not to open that door." footsteps approach, door creaks, low hum]
```

- Dialogue inside double quotes = lip-synced speech
- Everything else = ambient SFX
- Max 2 speakers per 10s clip
- Use `[whisper]`, `[shout]`, `[echo]` inside quotes for emphasis

## Quick Comparison Notes

### MAGI-2 vs Kling 3.0
- **MAGI-2 wins**: Native audio (Kling 3.0 has no single-pass audio), omni-reference (20 vs 0), lip-sync precision
- **Kling 3.0 wins**: Faster inference on consumer GPUs, lower cost per second on hosted
- **Trade-off**: Kling 3.0 excels at short (4–8s) high-motion clips; MAGI-2 handles longer narrative sequences with consistent characters

### MAGI-2 vs Seedance 2.5
- **MAGI-2 wins**: Multi-shot continuity (Seedance 2.5 resets on scene change), 30s max clip (vs 12s), audio sync
- **Seedance 2.5 wins**: Higher max resolution (4K upscale), better physics simulation for liquids/particles
- **Trade-off**: Seedance 2.5 for VFX-heavy shots; MAGI-2 for dialogue-driven storytelling

## Troubleshooting Tips

| Issue | Fix |
|---|---|
| Audio out of sync | Keep dialogue under 15 words per speaker, avoid overlapping speech |
| Character inconsistency | Use same `[SUBJECT]` across all clips, include 1–2 omni-reference images |
| Motion blur artifacts | Reduce action complexity: "walks" → "steps slowly" |
| Loss of style after clip change | Copy exact `[STYLE]` block, add `[transition: crossfade 0.5s]` |
| 480B fails to generate | Fallback to 114B preview (Apache-2.0 weights) via failover routing |

## Copyable Prompt Templates

**For hosted 480B (with audio):**
```
[SCENE: dimly lit Victorian study, fireplace crackling, bookshelves floor-to-ceiling]
[SUBJECT: a woman in a velvet gown, holding a letter, trembling hands]
[ACTION: reads letter aloud, eyes widen, drops it]
[STYLE: warm candlelight, soft focus, 24fps film grain]
[AUDIO: "How dare they." fire pops, paper rustles, clock ticks]
```

**For open-source 114B (no audio, shorter):**
```
[SCENE: desert highway at sunset, dust storm approaching]
[SUBJECT: a lone motorcyclist, leather jacket, goggles up]
[ACTION: revs engine, accelerates into the storm]
[STYLE: wide shot, golden hour, anamorphic]
```

**Multi-shot chain (480B only):**
```
[SCENE: hospital corridor, fluorescent lights, linoleum floor] [SUBJECT: a doctor in scrubs, stethoscope, tired eyes] [ACTION: walks past empty nurse station, stops, looks at clipboard] [STYLE: clinical, desaturated, handheld] [AUDIO: footsteps echo, distant beeping, paper rustles] [TRANSITION: cut to next shot]
[SCENE: same corridor, now dark, emergency lights red] [SUBJECT: same doctor, now holding a bloody bandage] [ACTION: runs toward camera, breathing heavy] [STYLE: same, but with red emergency lighting] [AUDIO: alarm blares, "Code blue!" shouted, running footsteps]
```
