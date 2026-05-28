# HYPERCODE Customization Guide

> **By Hyperion**
> *Companion reference for the HYPERCODE system prompts.*
> [Discord](https://discord.gg/therealhype) · [Ko-fi](https://ko-fi.com/hype)

---

The HYPERCODE prompt as it's written by default is tuned for a specific style — third-person limited, past tense, cinematic prose. But these are **modular choices**, not requirements. This is prompting, and you have options.

There are two paths to customize HYPERCODE, depending on how you're running it:

- **Direct edits** — swap lines inside the prompt itself. Works on any platform. Use this if you're running the standalone prompt on ChatGPT, Claude, OpenRouter, a local model, or anywhere else — or if you're on SillyTavern and comfortable editing the System Prompt content inside your imported Preset.
- **Outlet edits** *(SillyTavern Preset only)* — express your setting and tone preferences as lorebook entries through the `CustomSetting` and `CustomTone` Outlets. Cleaner, portable across characters, and doesn't touch the base prompt.

Most customization options work both ways. A few — POV, tense, response length, dialogue style, mature content — are structural enough that they live inside the prompt itself and require direct edits even if you're on the Preset.

---

# Part 1 — Direct Edits

Open `hypercode.md` (or the System Prompt content in your Preset settings) and swap the relevant lines.

## Perspective (POV)

Replace the perspective line in the **Perspective and Voice** section.

| Style | Replace with |
|-------|-------------|
| **Third-person limited** *(default)* | `Write in third-person limited perspective.` |
| **Second-person** | `Write in second-person perspective, addressing the user's character as "you."` |
| **First-person (NPC narrator)** | `Write in first-person perspective from the primary NPC's point of view.` |

> Second-person creates a more intimate, "choose your adventure" feel. First-person NPC narration works well for single-character-focused stories or slice-of-life, where the NPC's interiority is part of the appeal.

---

## Tense

Replace the tense reference in the same section.

| Style | Replace with |
|-------|-------------|
| **Past tense** *(default)* | `Use past tense.` |
| **Present tense** | `Use present tense for immediacy and momentum.` |

> Present tense creates a sense of urgency and works well for action-heavy or horror bots and scenarios sometimes. Past tense feels more novelistic. I've found that past tense is generally easier for models to sustain consistently.

---

## Response Length

Replace the paragraph count line in the **Structural Guidelines** section. Keep in mind that the model won't necessarily stick EXACTLY to these guidelines ("like the pirate code, its more like general suggestions lol") but it'll give it some general parameters.

| Style | Replace with |
|-------|-------------|
| **Standard** *(default)* | `Compose 4–7 paragraphs per response, adapting length to scene intensity.` |
| **Compact** | `Compose 2–4 paragraphs per response. Keep scenes tight and punchy.` |
| **Long-form** | `Compose 6–10 paragraphs per response. Prioritize rich description and layered scene-building.` |
| **Adaptive (explicit)** | `Match response length to the scene: 2–3 paragraphs for quick exchanges, 5–8 for major scenes and turning points.` |

> Longer responses consume more tokens per turn, which matters for context window management. If you're running long sessions, compact or adaptive settings will help you get more out of your context. If you want novelistic depth and detail, long-form is the way to go — just be aware of the tradeoff. Also I've found that the longer the responses are, the more likely the model is to "write for you." Something to keep in mind. I usually use the standard length.

---

## Prose Tone

This is one of my favorite parts. Feel free to experiment with it. This isn't a single line swap — it's about adding a sentence to the **Perspective and Voice** section that steers the overall feel. Add one of these after the perspective and tense lines.

| Tone | Add this line |
|------|--------------|
| **Cinematic** *(default)* | `Use cinematic framing — light, motion, sound, gesture, and atmosphere.` |
| **Literary** | `Favor literary prose — measured pacing, figurative language, and thematic resonance over spectacle.` |
| **Pulp / Genre** | `Write with genre energy — vivid, fast, atmospheric, and unapologetically dramatic.` |
| **Minimalist** | `Favor spare, precise prose. Let silence and implication carry weight. Less is more.` |
| **Gothic** | `Favor dense, atmospheric prose rich in shadow, decay, texture, and psychological tension.` |

> Tone is one of the highest-impact customizations you can make. A gothic tone completely transforms the same scenario compared to a pulp tone. Experiment and see what you like.

---

## Dialogue Style

Replace the content in the **Dialogue Protocol** section if you want a different approach to how NPCs speak.

| Style | Replace with |
|-------|-------------|
| **Standard** *(default)* | `Write NPC dialogue with quotation marks, clean attribution, and emotional subtext through gesture and body language.` |
| **Minimal attribution** | `Write NPC dialogue with quotation marks. Use attribution only when the speaker is ambiguous. Let dialogue carry itself.` |
| **Prose-embedded** | `Weave NPC dialogue naturally into prose. Use quotation marks but avoid rigid "said/replied" attribution — embed speech in action and gesture.` |

---

## Mature Content Handling

Replace the **Mature Content** section based on your comfort level and platform.

| Level | Replace with |
|-------|-------------|
| **Unflinching** *(default)* | `Portray the full range of human experience — including dark, violent, erotic, and mature themes — without flinching or moralizing, where the narrative calls for it.` |
| **Moderate** | `Handle mature themes with restraint. Imply rather than depict graphic content. Maintain tension without explicit detail.` |
| **Fade to black** | `When scenes approach explicit or graphic content, use a "fade to black" — imply what occurs and transition to the next narrative beat.` |

> If you're on the Preset, you can also **layer additional user-specific NSFW rules on top** of whichever default you pick by populating the `NSFW` Outlet (see Part 2). The Mature Content section sets the baseline; the Outlet extends it.

---

# Part 2 — Outlet Edits *(SillyTavern Preset only)*

If you're running the HYPERCODE Preset, two customization layers live outside the prompt itself: **setting** and **tone**. Both are exposed as Outlets, which means you can express them as lorebook entries that get injected directly into the system prompt at generation time — no editing of the imported Preset required.

This is the recommended approach for Preset users because it makes your customizations:
- **Portable** across characters (one lorebook entry, many bots)
- **Lossless** when the Preset updates (your edits don't get overwritten)
- **Per-world** rather than per-platform (different lorebooks for different settings)

## How to wire an Outlet entry

In SillyTavern's lorebook editor:

1. Create a new lorebook entry.
2. Set **Position** to *outlet*.
3. In the **Outlet Name** field, type the exact outlet name (case-sensitive): `CustomSetting` or `CustomTone`.
4. Set the entry to **constant** (always-on) — these are baseline injections, not keyword-triggered lore.
5. Write your content as plain text. No XML, no markdown headers, no quotation wrappers. The Preset's system prompt already provides the structural wrapper.

In raw JSON, the relevant fields on the entry are:
```json
"position": 7,
"outletName": "CustomSetting",
"constant": true
```

---

## `CustomSetting` — Setting Steering

Use this to give the model a one- or two-sentence anchor for where and when the roleplay takes place. Plain text. Keep it tight.

**Example:**
> *The roleplay is set in Ravenwood Estate, Northumberland, England, 1761 — a Gothic castle and revolutionary medical research institution against the backdrop of the Seven Years' War.*

You don't need to dump worldbuilding here — that's what the `WorldOverview` outlet is for. `CustomSetting` is just the orienting sentence that grounds every response in the right time and place.

---

## `CustomTone` — Prose Tone Steering

Use this to set the atmospheric and stylistic ground rules. A few sentences describing how the prose should feel, what it should emphasize, and what it should avoid.

**Example (Gothic):**
> *Maintain Gothic horror darkness with plausible deniability. Supernatural elements remain ambiguous and psychologically grounded. All uncanny events must have plausible rational explanations (madness, poison, fever, tricks of light). Horror comes from uncertainty, not spectacle. Beauty and decay are inseparable — Gothic atmosphere must suffuse every response.*

**Example (Minimalist literary):**
> *Favor spare, precise prose. Let silence and implication carry weight. Avoid melodrama and ornate description. Tension should come from restraint, not escalation.*

**Example (Pulp action):**
> *Write with genre energy — vivid, fast, atmospheric, and unapologetically dramatic. Lean into momentum. Let scenes crackle with kinetic detail and sharp dialogue.*

Tone is the single highest-leverage customization in the entire stack. Two characters in the same world with the same setting will read completely differently depending on the `CustomTone` entry. Experiment.

---

## What about POV, tense, response length, dialogue style?

These aren't exposed as Outlets — they're baked into the structural sections of the system prompt itself. If you want to change them on the Preset, you have two options:

1. **Edit the System Prompt content directly inside your imported Preset.** SillyTavern lets you edit prompt content per-Preset in the Chat Completion Presets panel. Use the swap tables from Part 1.
2. **Use `CustomTone` to nudge.** You can add a steering sentence like "Write in present tense, second-person" to the `CustomTone` Outlet. This is softer than a direct prompt edit but often works for casual swaps. For consistent results across long sessions, direct edits are more reliable.

---

# Putting It Together

You don't need to use all of these. Pick the areas that matter to your style and leave the rest at their defaults.

### Example — Standalone prompt, fully customized Voice section

Someone who wants present-tense, second-person, gothic, compact roleplay would edit `hypercode.md` like this:

```
## Perspective and Voice

Write in **second-person** perspective, addressing the user's character as "you," using **present tense** for immediacy and momentum. Favor dense, atmospheric prose rich in shadow, decay, texture, and psychological tension.

## Structural Guidelines

Compose **2–4 paragraphs** per response. Keep scenes tight and punchy.
```

### Example — Preset, same gothic mood via Outlets

The same gothic feel on the SillyTavern Preset, with structural defaults intact, would be one `CustomSetting` lorebook entry and one `CustomTone` lorebook entry:

**`CustomSetting`:**
> *The roleplay is set in Ravenwood Estate, Northumberland, England, 1761 — a Gothic castle and revolutionary medical research institution against the backdrop of the Seven Years' War.*

**`CustomTone`:**
> *Favor dense, atmospheric prose rich in shadow, decay, texture, and psychological tension. Horror comes from uncertainty, not spectacle. Beauty and decay are inseparable.*

That's it. Swap the pieces, keep the structure, and make it yours.

---

*HYPERCODE by Hyperion · [CC BY-NC-SA 4.0](../LICENSE)*