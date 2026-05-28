<!-- Banner placeholder — replace with your brand image -->
<!-- Recommended size: 1280x640px or similar 2:1 ratio -->
![HYPERCODE Banner](assets/banner.png)

# HYPERCODE

**A premium roleplay prompt architecture by Hyperion.**

HYPERCODE is a literary-quality system prompt for immersive, cinematic AI roleplay — released in two forms: a **standalone prompt** that drops into any frontend on any model, and the **HYPERCODE Preset for SillyTavern**, which wraps the same prompt in a fully modular prompt stack with documented injection points called Outlets.

[![Ko-fi](https://img.shields.io/badge/Support%20on-Ko--fi-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/hype)
[![Discord](https://img.shields.io/badge/Join%20the-Discord-5865F2?logo=discord&logoColor=white)](https://discord.gg/therealhype)
[![Substack](https://img.shields.io/badge/Read-The%20Hyperium-FF6719?logo=substack&logoColor=white)](https://hyperionblackthorne.substack.com)

---

## What's Included

Two deliverables, same prompt at the core, different surfaces around it.

| Form | File | Use This If |
|------|------|-------------|
| **Standalone Prompt** | [`hypercode.md`](prompts/v1.0/hypercode.md) | You're on any frontend or model that takes a system prompt — ChatGPT, Claude, OpenRouter, local models, mobile clients, anything. |
| **HYPERCODE Preset** | [`HYPERCODE-1.0.json`](presets/v1.0/HYPERCODE-1.0.json) | You're on SillyTavern and want the full prompt stack with Outlet-based modular customization. |

Both ship the same core narrative voice. The Preset adds structural scaffolding and runtime injection slots that the standalone can't express on its own.

---

## Quick Start

### Standalone (any platform)
1. Open [`prompts/v1.0/hypercode.md`](prompts/v1.0/hypercode.md).
2. Copy the contents of the fenced code block.
3. Paste into your platform's system prompt field.
4. Optionally swap in any of the customization options from the [Customization Guide](guides/customization.md).

### SillyTavern Preset
1. Download [`presets/v1.0/HYPERCODE-1.0.json`](presets/v1.0/HYPERCODE-1.0.json).
2. In SillyTavern, open the **API Connections** panel and find the **Chat Completion Presets** dropdown.
3. Click the import icon and select the JSON file.
4. Select **HYPERCODE** from the preset dropdown.
5. (Recommended) Build a character lorebook with Outlet entries — see the [Outlets](#outlets) section below.

---

## The Prompt Stack

HYPERCODE isn't a single block of text. It's a **stack** — layered prompt fragments assembled in a fixed order so that the model sees character, world, lore, history, and runtime instructions arranged for maximum coherence and recency.

In the Preset, the assembled order looks like this:

```
   ╭─── IDENTITY ───────────────────────────────────────────╮
   │  System Prompt               ← HYPERCODE core          │
   │    ├─ <Custom_Setting>       ← Outlet: CustomSetting   │
   │    └─ <Custom_Tone>          ← Outlet: CustomTone      │
   ╰────────────────────────────────────────────────────────╯
                            │
                            ▼
   ╭─── WORLD & CHARACTER ──────────────────────────────────╮
   │  World Overview              ← Outlet: WorldOverview   │
   │  World Info (before)                                   │
   │  Character Description                                 │
   │  Character Personality                                 │
   │  Scenario                                              │
   │  Persona Description                                   │
   │  Persona Lorebook            ← Outlet: persona         │
   │  World Info (after)                                    │
   ╰────────────────────────────────────────────────────────╯
                            │
                            ▼
   ╭─── MEMORY & HISTORY ───────────────────────────────────╮
   │  Chat Lore                   ← Outlet: LTM             │
   │  Chat Examples                                         │
   │  Chat History                                          │
   ╰────────────────────────────────────────────────────────╯
                            │
                            ▼
   ╭─── RUNTIME & SAFETY ───────────────────────────────────╮
   │  Commands                    ← Outlet: command         │
   │  NSFW                        ← Outlet: NSFW            │
   │  System Note                 ← Final reinforce         │
   ╰────────────────────────────────────────────────────────╯
```

### Why It's Built This Way

- **Separation of concerns.** Identity sets the voice. World & Character grounds the scene. Memory & History anchors continuity. Runtime & Safety reinforces the rules at the moment of generation. Each band is editable without breaking the others.
- **Layered overrides.** Defaults are baked in. User-specific customizations layer on top via Outlets without ever touching the base prompt.
- **Late-stack reinforcement.** Critical guardrails — narrator identity, content rules, anti-fourth-wall reminders — sit near the end of the stack so the model encounters them closest to its generation moment. Recency bias is leveraged on purpose.
- **Portability.** Because lore lives in the lorebook (not the prompt), the same Preset works across hundreds of characters and worlds without modification. The prompt is the chassis; the lorebook is the cargo.

---

## Outlets

Outlets are SillyTavern's prompt-injection slots — named hooks the Preset exposes so your lorebook entries can be dropped into specific points of the assembled stack. To wire a lorebook entry to an Outlet, set the entry's **Position** to *outlet* and the **Outlet Name** field to the exact name listed below.

> **CRITICAL: OUTLETS ARE CASE-SENSITIVE. I.E., you have to put `CustomSetting`, not `customsetting` when you are assigning your lorebook entries. Also, outlets will not appear to be populated on the prompt section of your ST setup. It'll just say which one it is (like {{outlet::CustomSetting}}). But the outlet will be populated in the prompt that is sent to the AI at runtime. You can use Prompt Inspector to check and make sure if you're doubtful.**

Outlets are optional. An unwired Outlet renders as inert — the stack continues to work normally. But populating them is where the Preset earns its keep.

### `CustomSetting`
**What it's for:** A 1–2 sentence description of your roleplay's setting (where, when, what kind of world). Plain text only — no XML, no markdown headers.
**Where it lands:** Inside the system prompt's `<Custom_Setting>` block, near the top of the stack.
**Example content:**
> *The roleplay is set in Ravenwood Estate, Northumberland, England, 1761 — a Gothic castle and revolutionary medical research institution against the backdrop of the Seven Years' War.*

### `CustomTone`
**What it's for:** A few sentences describing the prose tone, mood, and atmospheric ground rules you want the model to maintain. Plain text only.
**Where it lands:** Inside the system prompt's `<Custom_Tone>` block, near the top of the stack.
**Example content:**
> *Maintain Gothic horror darkness with plausible deniability. Supernatural elements remain ambiguous and psychologically grounded. Horror comes from uncertainty, not spectacle.*

### `WorldOverview`
**What it's for:** A full, structured overview of your world — setting, premise, genre, factions, themes, technological limits, etc. This is the high-altitude lore the model needs before scene-level entries fire.
**Where it lands:** Position #2 in the stack, immediately after the system prompt.
**Tip:** One per world. Keep it under ~1000 tokens. Use headed sections (Setting, Premise, Atmosphere, Themes, etc.) rather than freeform paragraphs.

### `persona`
**What it's for:** Additional lore specific to the user's persona — backstory, relationships, identity details, anything the model should know about *who the user is* in this world.
**Where it lands:** Right after the standard Persona Description block, allowing layered persona context.
**Tip:** This is useful if you have larger persona lorebooks you keep attached to your user personas. Use the `persona` outlet for each entry in your persona lorebook and it'll keep these entries arranged in a logical section of the prompt stack.

### `LTM`
**What it's for:** Long-term memory — summaries of past events, established relationships, prior scenes the model should treat as history. The "previously, on this story" injection.
**Where it lands:** Just before chat history, framed by the Preset as established past events.
**Tip:** Use a summary extension or use manual summaries to populate a Chat Lore Lorebook. Set your Chat Lore Lorebook entries to the `LTM` outlet and order them in sequence and they'll fall right into place.

### `command`
**What it's for:** Runtime command definitions — custom `!` commands you can invoke mid-chat to trigger temporary behavioral modifications (perspective shifts, scene jumps, character swaps, etc.).
**Where it lands:** Late in the stack, just before NSFW and System Note, so commands are fresh in the model's recent context.

### `NSFW`
**What it's for:** User-supplied custom character-specific NSFW rules, kinks, hard limits, or content steering that should override or extend the default Mature Content section in the system prompt.
**Where it lands:** Near the end of the stack — high-recency for the model.
**Tip:** You can also set your NSFW preferences or boundaries in persona-bound lorebooks or just a global preferences lorebook. Set the entry to the `NSFW` outlet and the model will get the memo. This is also great for building character lorebooks for characters with specific NSFW definitions. Set those entries in their character lorebook to this outlet and make it keyword triggered to fire at the right time.

---

## Customization

The [**Customization Guide**](guides/customization.md) covers two paths to the same destination:

- **Direct edits** — line-swap replacements for POV, tense, response length, prose tone, dialogue style, and mature content handling. Use this if you're on the standalone prompt.
- **Outlet edits** — the same customization surface, but expressed as lorebook entries through `CustomSetting` and `CustomTone`. Use this if you're on the Preset.

Either way, the customization model is the same: swap the pieces you care about, keep the rest at their defaults.

---

## Versions

The prompt and the Preset are versioned **independently**.

| Component | Version | Status | Notes |
|-----------|---------|--------|-------|
| HYPERCODE Prompt | [v1.0](prompts/v1.0/hypercode.md) | **Current** | Bumps to v1.1 only when the prompt content itself changes. |
| HYPERCODE Preset | [v1.0](presets/v1.0/HYPERCODE-1.0.json) | **Current** | First public release of the Preset. |

### What's New in This Release

- **HYPERCODE Preset launches** — full SillyTavern preset with seven Outlets and a documented assembled stack order.
- **Premium → HYPERCODE.** The prompt formerly known as "Premium" is now simply **HYPERCODE**, and is the actively maintained reference prompt going forward.
- **Essentials and Core archived.** Both have been moved to [`prompts/v1.0/archive/`](prompts/v1.0/archive/). They remain available for reference and use, but will not receive further updates.

---

## The Full Experience

HYPERCODE is one piece of a larger ecosystem. If you want to experience what these prompts can really do when paired with handcrafted characters, deep worldbuilding, and a curated community — check below:

**Timeless Tavern** is a SillyTavern instance hosted by me, Hyperion. Multi-user, multi-world, and running on a prompt architecture that goes well beyond what's published here. Access is through the [**Discord**](https://discord.gg/therealhype).

## Other Ways to Connect

- **Wyvern** — [Hyperion](https://app.wyvern.chat/profiles/hyperion) — Check out some of my characters.
- **Discord** — [Hype Discord](https://discord.gg/therealhype) — Community, support, and access to Timeless Tavern.
- **Ko-fi** — [ko-fi.com/hype](https://ko-fi.com/hype) — Support the work. Memberships and commissions available.
- **The Hyperium** — [Substack](https://hyperionblackthorne.substack.com) — Writing, worldbuilding, and studio updates.
- **Tumblr** — [@hyperionblackthorne](https://hyperionblackthorne.tumblr.com) — AI art and dark aesthetic.

---

## License

HYPERCODE is released under [CC BY-NC-SA 4.0](LICENSE). You're free to use, share, and adapt it with attribution — just not commercially. See the license file for full terms.

## Contributing

Found something that could be better? Have a suggestion for the Customization Guide or the Outlets reference? Open an issue or submit a PR. Community contributions that improve the framework are welcome.

If you build something cool with HYPERCODE, I'd love to hear about it — drop into the [Discord](https://discord.gg/therealhype) and share.

---

<p align="center">
  <a href="https://ko-fi.com/hype"><img src="https://img.shields.io/badge/Support%20on-Ko--fi-FF5E5B?logo=ko-fi&logoColor=white&style=for-the-badge" alt="Support on Ko-fi"></a>
  &nbsp;&nbsp;
  <a href="https://discord.gg/therealhype"><img src="https://img.shields.io/badge/Join%20the-Discord-5865F2?logo=discord&logoColor=white&style=for-the-badge" alt="Join the Discord"></a>
</p>

<p align="center"><em>Crafted by Hyperion</em></p>