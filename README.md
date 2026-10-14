# Code Animation (`/codeanimation`)

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

## Installing as an Agent Skill

### Antigravity
Symlink into Antigravity's global skills directory:
```bash
ln -s ~/dev/skills/codeanimation ~/.gemini/config/skills/codeanimation
```

### Claude Code / OpenCode / General Agent Rigs
Symlink into your user agents skills directory:
```bash
ln -s ~/dev/skills/codeanimation ~/.agents/skills/codeanimation
```

Once installed, invoke the skill in chat with **`/codeanimation`** or by asking the agent to create an animated video.

---

## Quick Start (Standalone / CLI)

### 1. Install Dependencies
```bash
git clone https://github.com/your-username/codeanimation.git
cd codeanimation
npm install
```

### 2. Scaffold a New Animation Project
Create a self-contained project anywhere on your system:
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
