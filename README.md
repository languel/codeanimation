# Code Animation (`/codeanimation`)


> Experimental and unpolished. These are classroom experiments, not a supported production toolkit. Expect rough edges, changing dependencies, and different results across models and harnesses.

A framework and agent skill for creating **hand-painted, code-driven 2D animated videos and motion graphics** using [p5.js](https://p5js.org) and [p5.brush](https://github.com/acamposuribe/p5.brush).

The aesthetic is physical and organic: boiling ink linework, watercolor washes that bleed into the paper grain, additive glows, and calm eased motion. Every frame is a deterministic pure function of time `t`, allowing instant seeking in an interactive browser studio and fast parallel rendering to MP4 via headless Chrome and FFmpeg.

---

## Highlights

- **Harness & Model Agnostic:** Works in any AI coding environment (Antigravity, Claude Code, Cursor, OpenCode, Codex) or directly from the terminal with standard Node.js.
- **Any Subject:**
  - **Narrative Cartoons & Characters:** Rig characters with drawn key views (`front`, `3/4`, `side`, `back`), squashing, facial acting, emotes, and prop attachment hooks.
  - **Technical Explainers:** Visualize data pipelines, network architectures, algorithms, and scientific concepts with tactile watercolor nodes, glowing packets, and conduits.
  - **Motion Graphics & Loops:** Create seamless 4-second looping backgrounds, animated badges, and visualizers.
- **Any Music or Audio Track:** Lock movement and cutting to song BPM, calculate beat progress, trigger musical accents, and act out lyrics without cluttering the screen with text.
- **Any Aspect Ratio:**
  - `16:9` Widescreen (1920×1080) for desktop & YouTube.
  - `9:16` Vertical (1080×1920) for TikTok, Reels, and YouTube Shorts (with safe zone rules).
  - `1:1` Square (1080×1080) for social feeds and cards.
- **Visual Review Loop:** High-speed contact sheets (`--sheet`), frame strips (`--strip`), and detail crops (`--crop`) let you inspect timing, poses, and contacts without watching full-length renders.

---

## Prerequisites

- **Node.js** `>= 20.10`
- **Google Chrome** or **Chromium** (for headless frame capture)
- **FFmpeg** (for MP4 encoding and audio muxing)

---

## Inspiration and credit

This skill was inspired by [JohnHeibel/ClaudeAnimationBase](https://github.com/JohnHeibel/ClaudeAnimationBase), John Heibel’s starter kit for hand-painted animation with p5.js and p5.brush. Thank you to John for sharing the original kit and animation guidance. This repository adapts that inspiration into an experimental skill for students using different coding-agent harnesses.

## Installation

This repository contains **one skill**, `codeanimation`. It follows the [Agent Skills standard](https://agentskills.io): a folder with `SKILL.md` and supporting resources. Claude Code supports this standard too; the discovery folder differs. `AGENTS.md` project instructions and subagent definitions are separate concepts.

Install the whole folder, not just `SKILL.md`. Installing a skill makes its instructions available; it does not install Node.js, Chrome, FFmpeg, or the generated project's npm dependencies.

### Clone once

The commands below use macOS/Linux shell syntax. On Windows, copy the complete folder into the corresponding skills directory, or use WSL.

```bash
mkdir -p ~/dev/skills
git clone https://github.com/languel/codeanimation.git ~/dev/skills/codeanimation
```

If already cloned, keep that checkout and substitute its absolute path below.

### Harnesses with shared `.agents` paths

[Codex](https://developers.openai.com/codex/skills/), [OpenCode](https://opencode.ai/docs/skills/), [Gemini CLI](https://geminicli.com/docs/cli/skills/), and [Cursor](https://cursor.com/docs/skills) support `.agents/skills` in projects and `~/.agents/skills` for personal installs.

```bash
mkdir -p ~/.agents/skills
ln -s ~/dev/skills/codeanimation ~/.agents/skills/codeanimation
```

### Claude Code

[Claude Code](https://code.claude.com/docs/en/skills) supports standard Agent Skills and documents `.claude/skills` for discovery. Use its documented path rather than assuming `.agents` discovery:

```bash
mkdir -p ~/.claude/skills
ln -s ~/dev/skills/codeanimation ~/.claude/skills/codeanimation
```

OpenCode and Cursor also recognize Claude-compatible paths. If you use both groups, the links can point to the same checkout; avoid duplicate installs in a harness if it reports name conflicts.

### Antigravity

[Antigravity](https://antigravity.google/docs/skills) uses `.agents/skills` for project installs (legacy `.agent/skills` is also supported). Its app and IDE use this global path:

```bash
mkdir -p ~/.gemini/config/skills
ln -s ~/dev/skills/codeanimation ~/.gemini/config/skills/codeanimation
```

For Antigravity CLI, use `~/.gemini/antigravity-cli/skills` instead.

### Install just this skill in a project

From your project's root, copy the complete skill folder:

```bash
mkdir -p .agents/skills
cp -R ~/dev/skills/codeanimation .agents/skills/codeanimation
```

For Claude Code, substitute `.claude/skills` for `.agents/skills`. Copy into a new destination to avoid merging an old install. A copy is portable when sharing the project; an absolute symlink depends on your machine. You may omit the copied `.git` directory: it is repository metadata, not part of the skill.

This is an individual-skill install: no other skills are bundled or required. For other harnesses that implement Agent Skills, copy this folder into their documented discovery location. A tool that only reads `AGENTS.md` does not automatically support skill discovery; explicitly ask it to read `SKILL.md` and follow the linked resources.

### Use, update, and uninstall

Start a new session or reload skills after installation. In Codex, use `$codeanimation`; in Claude Code, use `/codeanimation`. Other harnesses may offer slash commands, a skill picker, or automatic selection. A portable request is: "Use the codeanimation skill to make a short animation about …".

Update the source checkout with `git -C ~/dev/skills/codeanimation pull --ff-only`. Symlink installs follow that checkout; copied installs need to be refreshed. To uninstall a symlink, remove only the link (for example `unlink ~/.agents/skills/codeanimation`).

Discovery paths were checked against the linked official documentation on October 14, 2026. This does not mean the rendering pipeline has been tested in every harness.

Once installed, you can invoke the skill directly in chat:
- **Scaffold a new project:** `/codeanimation scaffold ./my-video --aspect=9:16 --duration=15`
- **Storyboard & animate:** `/codeanimation A curious robot discovering a glowing mushroom in the forest`
- **Review frames:** `/codeanimation review sheet 0.5,1.5,2.5,3.5`
- **Render to MP4:** `/codeanimation render`

---

## Quick Start (Standalone / CLI)

### 1. Clone the skills monorepo
```bash
git clone https://github.com/languel/codeanimation.git
cd codeanimation
```

### 2. Scaffold and Install a New Animation Project
```bash
node kit/scaffold.mjs ./my-first-video --aspect=16:9 --bpm=120 --duration=10
cd my-first-video
npm install
```

### 3. Inspect in Interactive Studio
Open `studio.html` in Chrome to scrub through the timeline interactively, or launch a local server:
```bash
npx serve .
# Open http://localhost:3000/studio.html
```

### 4. Visual Review Loop
Generate contact sheets to inspect storytelling poses and transitions before rendering video:
```bash
# Check key storytelling moments across the video:
node render.mjs --sheet=0.5,1.5,2.5,3.5 --cols=4 --w=360 --out=out/check/sheet.jpg

# Inspect every frame of a specific moment (e.g. a jump or turn):
node render.mjs --strip=1.8:2.4 --cols=6 --w=320 --out=out/check/strip.jpg

# High-resolution close-up on faces or prop contacts:
node render.mjs --sheet=2.0 --crop=860,650,400,400 --w=600 --out=out/check/crop.jpg
```

### 5. Render to MP4
```bash
# Direct video render:
node render.mjs --clip --out=out/video.mp4

# Parallel frame rendering with multi-core workers:
node render.mjs --frames --workers=4
node render.mjs --encode --audio=assets/song.mp3 --out=out/video.mp4
```

---

## Directory Structure

```text
codeanimation/
├── SKILL.md                          # Master agent skill prompt & guidelines
├── package.json                      # p5, p5.brush, puppeteer-core
├── kit/
│   ├── core.js                       # Canvas setup, painting, camera, boilSeed, glow, paper
│   ├── timeline.js                   # Pure t-function shot sequencer & transition system
│   ├── config.js                     # Project settings (duration, bpm, dimensions, audio)
│   ├── audio.js                      # Rhythm, beat mapping, downbeat hits, musical impacts
│   ├── studio.html                   # Interactive browser scrubber with responsive canvas
│   ├── render.mjs                    # Puppeteer headless renderer & FFmpeg encoder
│   ├── scaffold.mjs                  # CLI project generator
│   ├── sheets.js                     # Model loops for visual inspection
│   └── characters/
│       ├── template.js               # Generic character/object rigging blueprint
│       └── actor.js                  # Reference character rig (31 emotions, views, dances)
├── references/
│   ├── craft.md                      # 12 principles of animation in code, timing & reads
│   ├── characters.md                 # Character design, key views, face rigs & attachments
│   ├── music-and-audio.md            # BPM sync, rhythm choreography, acting out lyrics
│   ├── explainers.md                 # Technical, algorithmic, and data flow visualizations
│   └── formats.md                    # 16:9 widescreen, 9:16 vertical shorts, and 1:1 square
└── examples/
    ├── demo-character.js             # Narrative cartoon example ("The Fallen Star")
    ├── demo-music-beat.js            # Beat-synchronized musical performance example
    └── demo-explainer.js             # Technical data pipeline explainer example
```

---

## Core Animation Principles

1. **The Medium is Solid 2D:** Use `paint()`, `inkLine()`, and `glow()`. Avoid flat digital flash shapes or fake 3D perspective boxes.
2. **Boiling Linework:** The engine reseeds line wobble 12 times a second (`BOIL = 12`). Always call `boilSeed(key)` before each separate visual element to prevent moving objects from causing static scenery to jitter.
3. **No Text Subtitles:** Act out dialogue and lyrics through pantomime, facial acting, visual metaphors, or hand-painted comic marks (`sfx()`, `letter()`).
4. **Model the Viewer's Eye:** Break every shot into sequential **reads**. Contrast quick actions with held reactions so the viewer has time to understand each moment.
5. **Transitions at Every Seam:** Always enter the first shot, bridge shot boundaries, and exit the final shot with an iris, brush wipe, pan, or cut on action.

---

## License

MIT
