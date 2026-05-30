# Gemini Omni — Complete Prompt Reference Guide

> **Source:** Synthesized from [deepmind.google/models/gemini-omni/prompt-guide/](https://deepmind.google/models/gemini-omni/prompt-guide/), the [Gemini Omni model page](https://deepmind.google/models/gemini-omni/), [Google Flow support docs](https://support.google.com/flow/), and the [Google AI Developer documentation](https://ai.google.dev/gemini-api/docs/).  
> **Last researched:** 2026-05-28  
> **Purpose:** Complete reference guide — use this file whenever generating or refining prompts for Gemini Omni and related Google AI models.

---

## Table of Contents

1. [What is Gemini Omni?](#1-what-is-gemini-omni)
2. [Five Core Prompt Elements (Official Framework)](#2-five-core-prompt-elements-official-framework)
3. [World Understanding — The Key Principle](#3-world-understanding--the-key-principle)
4. [Official Prompt Examples (Verbatim)](#4-official-prompt-examples-verbatim)
5. [Advanced Prompting Techniques](#5-advanced-prompting-techniques)
6. [Text Rendering in Video](#6-text-rendering-in-video)
7. [Multi-Modal Input Syntax](#7-multi-modal-input-syntax)
8. [Cinematic & Artistic Vocabulary Reference](#8-cinematic--artistic-vocabulary-reference)
9. [Negative Prompting](#9-negative-prompting)
10. [Veo 3 Prompt Guide (Comparison / When Veo is Used)](#10-veo-3-prompt-guide-comparison--when-veo-is-used)
11. [Gemini Image Generation — Nano Banana Prompt Guide](#11-gemini-image-generation--nano-banana-prompt-guide)
12. [Audio Generation — TTS Prompt Guide](#12-audio-generation--tts-prompt-guide)
13. [Audio Understanding Prompting](#13-audio-understanding-prompting)
14. [Text Prompting Best Practices (Gemini API)](#14-text-prompting-best-practices-gemini-api)
15. [Thinking Mode Reference](#15-thinking-mode-reference)
16. [API Parameters Quick Reference](#16-api-parameters-quick-reference)
17. [Safety, Watermarking & Access](#17-safety-watermarking--access)
18. [Quick Prompt Templates](#18-quick-prompt-templates)

---

## 1. What is Gemini Omni?

**Gemini Omni** (specifically **Gemini Omni Flash**) is Google DeepMind's flagship multimodal *creative generation* model, announced at **Google I/O 2026 (May 19, 2026)**. It is the first model where **Gemini's reasoning intelligence** is natively fused with **generative media creation** (video, audio, image, text).

> *"Gemini Omni is where Gemini's ability to reason meets the ability to create. It delivers a leap in world understanding, multimodality, and editing."*[^1]

### Core Modalities

| Modality | Input | Output | Notes |
|---|---|---|---|
| **Video** | ✅ | ✅ | 4s / 6s / 8s / 10s clips; editing existing videos |
| **Image** | ✅ | 🔜 Coming soon | Used as reference input ("Ingredients") |
| **Audio** | ✅ | ✅ | Music-reactive animation; voice sync |
| **Text** | ✅ | ✅ | Animated text overlays; conversational editing |
| **Objects** | ✅ | N/A | Object references for visual substitution |

> ⚠️ **Current scope:** Gemini Omni currently outputs **video**. Image and text outputs are coming. The model page says: *"We're starting with video outputs, and over time we'll enable image and text."*

### Access Platforms

| Platform | Description |
|---|---|
| **Gemini App** | Consumer creative use (Google AI subscription required) |
| **Google Flow** | *"An AI creative studio built with and for creatives"* — most powerful access |
| **YouTube Shorts** | Short-form video creation |

---

## 2. Five Core Prompt Elements (Official Framework)

These are the **five foundational building blocks** of an effective Gemini Omni prompt, as defined by the official prompt guide[^2]:

### 1. 🎥 Shot Framing and Motion

> *"How do you want to frame your shot? Wide-angle, medium, or close-up? How do you want your camera to move? Should it glide gently, or rush suddenly? Experiment to find the right approach for your scene."*

**Key choices:**
- **Shot type:** `wide-angle` · `medium shot` · `close-up` · `extreme close-up` · `POV shot`
- **Camera movement:** `glide gently` · `rush suddenly` · `dolly in` · `slowly pushes in` · `continuous smooth shot` · `over the shoulder`

---

### 2. 🎨 Style

> *"How should your scene feel? Realistic, or cinematic? Grounded, or majestic? Tell Gemini Omni the effect you want to create, and leave the model to work out the details."*

**Key principle:** State the desired **effect**, not a step-by-step recipe. Omni handles execution.

**Examples of style descriptors:**
- `realistic` · `cinematic` · `grounded` · `majestic`
- `claymation` · `stop motion` · `skeuomorphism` · `sizzle reel`
- `film noir` · `cartoon` · `lo-fi` · `glitch aesthetics`
- `contemporary flat-media` · `risograph-like` · `isometric 3D`

---

### 3. 💡 Lighting

> *"How should your scene be lit? Where does the light come from – the sun, a streetlamp, or off-screen? What effect does it create? Is the lighting crisp, warm, or ethereal?"*

**Specify two things:**
1. **Light source:** `sun` · `streetlamp` · `off-screen` · `neon sign` · `window` · `golden-hour`
2. **Light quality:** `crisp` · `warm` · `ethereal` · `eerie green neon glow` · `soft golden-hour`

---

### 4. 📍 Location

> *"Where is your scene set? Tell Gemini Omni the landscape you imagine – like 'an alien landscape with clear, azure water'. But you don't need to describe every single little detail, as Omni will work with your overall intention."*

**Official example:** *"an alien landscape with clear, azure water"*

**Key principle:** Describe the **overall landscape intent** — not every prop. Omni uses world knowledge to fill in.

---

### 5. 🎬 Action

> *"What is happening in your scene? Who are the characters and objects? How are they moving and interacting?"*

**Include:**
- Who: characters and objects
- What: their movements
- How: their interactions

---

## 3. World Understanding — The Key Principle

This is the **most important differentiator** between Gemini Omni and other video models:

> *"With Veo, you need to share precise instructions to get the best results. But with Gemini Omni, you don't have to be as prescriptive with your prompt. Instead, tell Omni what you want to create – and watch the model's reasoning and world knowledge bring the details to life."*[^3]

### Omni vs. Veo Prompting Philosophy

| Aspect | Gemini Omni | Veo 3.1 |
|---|---|---|
| **Prompt style** | Conversational, intent-based | Descriptive, prescriptive |
| **Level of detail** | Overall intention — model fills in details | Detailed frame-by-frame descriptions |
| **Editing** | Multi-turn iterative: *"Make the violin invisible"* | Single-generation instructions |
| **World knowledge** | Model reasons and infers | Must be stated explicitly |
| **Character reference** | `@CharacterName` / `@me` syntax | Describe character attributes |
| **Prompt length** | Can be short; world knowledge compensates | Longer = better results |

---

## 4. Official Prompt Examples (Verbatim)

All prompts below are directly from official Google DeepMind and Google Flow documentation[^4][^5].

### 4.1 World Understanding + Visual Style

```
Explain the difference between regular computing and quantum computing.
Visualize this sentence using a contemporary flat-media style that blends
minimalist vector shapes with rich organic textures. The aesthetic is defined
by a high-contrast, "electric" color palette of neon pinks, cyans, and limes
set against a deep navy background. A hallmark of this style is the use of
stipple shading and grainy gradients, which adds a tactile, risograph-like
quality to the otherwise simple geometric forms. By combining sharp edges with
these softened, speckled transitions, the illustration achieves a playful,
editorial feel.
```

*Demonstrates: Educational concept visualization + rich aesthetic specification + design vocabulary*

---

### 4.2 Text Rendering + Animation Timing (Sizzle Reel)

```
word by word, one word on a the screen at a time: did, you, know, that, this,
model, can, do, pretty, good, text!? each word appears with a different animated
style, perfect pacing to a rhythm, sizzle reel.
```

*Demonstrates: Word-level text control + rhythm-paced animation + sizzle reel genre*

---

### 4.3 Video Editing — Preserve + Add Effect

```
Edit this keeping everything the same. Add animated motion effects coming out of the skateboard.
```

*Demonstrates: Conversational edit + preservation instruction + object-specific effect*

---

### 4.4 Multi-Modal Input (Video + Image + Audio)

```
The birds from <video> loosely form the imperfect shape of a bird based on <image>.
They move to the music from <audio> and dissipate as they fly
```

*Demonstrates: Three input modality references in one prompt + semantic reasoning (birds forming bird shape) + audio-reactive motion*

---

### 4.5 Sequential Narrative from Story Image

```
Show me in this story. Follow the story exactly in order starting top left.
Entire story in 10 seconds. Cinematic
```

*Demonstrates: Image/storyboard reference + reading order instruction + duration control + style modifier + user personalization*

---

### 4.6 Complex Action — Video Editing (Hand Effect)

```
Make it look like the weird shape of my hand hole super zooms and magnifies
the ground it's looking at in sharper quality.
```

*Demonstrates: Spatial reasoning + zoom/magnify effect referencing input video*

---

### 4.7 Multi-Step Iterative Editing Sequence

```
Step 1: "Transport the violinist to the image environment"
Step 2: "Make the violin invisible"
Step 3: "Change the camera angle to be over the violinist's shoulder."
```

*Demonstrates: Omni's conversational, multi-turn video editing loop — each step refines the previous output*

---

### 4.8 Audio-Synchronized Animation

```
When the finger in <video> touches the animal toy play the sound the animal makes
```

*Demonstrates: Cross-modal trigger (visual action → audio event)*

---

```
The lights of the apartments start turning on in sync with the music.
```

*Demonstrates: Audio-reactive visual event (implicit audio input)*

---

### 4.9 Object Substitution

```
Change spaceship to <object>
```

*Demonstrates: Object reference substitution using `<object>` token*

---

### 4.10 Physics Simulation

```
A marble rolling fast on a chain reaction style track, continuous smooth shot
```

*Demonstrates: Physics-aware generation + camera continuity instruction*

---

### 4.11 Artistic Style + Negative Prompting (Claymation)

```
claymation explainer of protein folding, everything is made out of clay,
no hands, stop motion, accurate
```

*Demonstrates: Art medium directive + topic + negative prompt ("no hands") + accuracy requirement*

---

### 4.12 Complex Style + Multiple Negative Prompts

```
A skeuomorphism stop motion explainer about how the brain hippocampus works
with a compelling voiceover. Don't add seahorses. No voice cuts at the end.
Don't add text.
```

*Demonstrates: Named art style + voiceover request + three explicit negative constraints*

---

### 4.13 Highly Structured — 26-Letter Alphabet (Most Complex Example)

```
The video shows items of the alphabet. An unusual item starting with each letter
is shown sitting on a table (like a Capybara for C, disco globe for D and
Lava Lamp for L). All 26 letters must be represented by 26 items with matching
lower thirds displaying the letter. Only one item and lower third at a time.
Each lower third must look like a black marker written on a slip of paper in
the bottom left. Rapid fire, roughly 9 frames per item at 24FPS. Last frame is
a slip of paper 'THE END'. The whole video is accompanied by calm smooth music.
```

*Demonstrates: Multi-element sequence + technical specs (24FPS, 9 frames/item) + typography styling (lower thirds) + pacing + audio mood — the most technically sophisticated official example*

---

### 4.14 Ingredients Workflow (Google Flow — Most Powerful Technique)

Upload reference images as "ingredients," then describe their relationship in text[^6]:

**With ingredients of [a woman, a lava lamp, a foggy street]:**
```
The woman, whose torso is the lava lamp, walks down the foggy street.
```

**Tips for Ingredients:**
- Use images with **plain/segmented backgrounds** for best results
- Describe **how ingredients relate and interact**, not just what they are
- Works at 4s, 6s, 8s, 10s (Omni-exclusive vs. Veo 3.1's 8s-only limit)

---

## 5. Advanced Prompting Techniques

### 5.1 @-Mention Syntax (Google Flow)

Reference characters, avatars, and voices using `@` in the prompt box[^7]:

```
@CaptainZoro walking through a futuristic city.
@me dancing in the rain.
```

| Token | Purpose |
|---|---|
| `@CharacterName` | Reference a saved character from your character library |
| `@me` | Insert your personal avatar |
| `@Voice: [Name]` | Reference a saved voice profile for cross-scene consistency |

---

### 5.2 Agent Mode (Google Flow — Project-Level Prompting)

Enable **Agent mode** in Flow for conversational project-wide control[^8]:

- **Batch generation:** *"Give me 5 variations of this video with different lighting."*
- **Agent Instructions:** Set a persistent reference image + guidelines for the entire project
- **Capabilities:** Brainstorm, outline storyboards, develop mood boards, make plot recommendations
- **Session history** is kept per project

---

### 5.3 Use Gemini to Expand Your Prompt

> *"These tips will help you craft a great prompt, but you can also use Gemini to help you expand on the prompt and include more detail."*[^9]

**Workflow:**
1. Write your core idea as a rough prompt
2. Ask Gemini (chatbot) to expand and enrich it
3. Use the enriched prompt in Gemini Omni for generation

---

### 5.4 Complex Action Reference

> *"When you refer to a complex action Gemini Omni understands your intention, and how this action should be applied across your video. You don't need to describe it across every frame."*[^10]

Describe a complex action **once** — Omni applies it coherently throughout the video. No frame-by-frame instruction needed.

---

### 5.5 Conversational / Multi-Turn Editing

Unique to Gemini Omni Flash: **generate a video → edit with a natural language command → iterate**:

```
Generate → "Make the violin invisible" → "Change camera angle to be over shoulder" → "Transport to new environment"
```

No other major video model (Veo 3.1, Sora, Kling) supports this conversational video editing loop.

---

## 6. Text Rendering in Video

> *"Choose your type, placement, animation, and exposure. Gemini Omni doesn't just allow you to render text more accurately – but to create it in sync with your visuals."*[^11]

### Four Text Parameters

| Parameter | Description |
|---|---|
| **Type** | Font style (e.g., bold sans-serif, black marker, serif) |
| **Placement** | Where on screen (e.g., bottom left, lower third, center) |
| **Animation** | How text moves or appears (e.g., different animated style per word) |
| **Exposure** | Timing / duration of visibility |

**Key differentiator:** Text is not just rendered accurately — it is **synchronized with visual content**.

### Text Styling Examples from Official Prompts

| Style Description | From Prompt |
|---|---|
| Lower third as "black marker written on a slip of paper in the bottom left" | Alphabet video prompt |
| Individual word animations with "different animated style" | Word-by-word sizzle reel prompt |
| Last frame as "a slip of paper 'THE END'" | Alphabet video prompt |

---

## 7. Multi-Modal Input Syntax

Gemini Omni uses placeholder tokens for referencing multiple media inputs in a single prompt[^12]:

| Token | Type | Usage |
|---|---|---|
| `<video>` | Reference video | Embed a source/reference video |
| `<image>` | Reference image | Embed a reference image or scene environment |
| `<audio>` | Audio track | Reference music, sound effects, or voice |
| `<object>` | Physical object | Specify an object for visual substitution |

### Multi-Modal Prompt Patterns

**Three media types in one prompt:**
```
The birds from <video> loosely form the imperfect shape of a bird based on <image>.
They move to the music from <audio> and dissipate as they fly
```

**Object substitution:**
```
Change spaceship to <object>
```

**Video + action trigger → audio:**
```
When the finger in <video> touches the animal toy play the sound the animal makes
```

---

## 8. Cinematic & Artistic Vocabulary Reference

Use these terms in Gemini Omni prompts for precise creative direction[^13]:

### Camera / Shot Vocabulary

| Category | Terms |
|---|---|
| **Shot types** | `medium shot` · `wide shot` · `close-up` · `extreme close-up` · `POV shot` · `two-shot` · `single-shot` |
| **Camera moves** | `slowly pushes in` · `dolly shot` · `over the shoulder` · `continuous smooth shot` · `tracking shot` · `aerial view` · `worms eye` · `top-down shot` |
| **Technical specs** | `24FPS` · `rough 9 frames per item` · `8-second sequence` |

### Art Style / Aesthetic Directives

| Style | Usage |
|---|---|
| `claymation` | Everything made of clay; includes stop-motion feel |
| `stop motion` | Choppy, handcrafted animation feel |
| `skeuomorphism` | Realistic 3D representation of familiar objects |
| `sizzle reel` | Fast-paced promotional edit |
| `rapid fire` | Quick-cut sequence |
| `film noir` | High-contrast black and white; mysterious |
| `lo-fi` | Low-fidelity aesthetic; vintage feel |
| `glitch aesthetics` | Digital artifacts; corrupted visuals |
| `risograph` | Grainy, layered print aesthetic |
| `flat-media` | Minimalist vector shapes |
| `stipple shading` | Dotted texture for shading |

### Environment / Atmosphere

| Term | Effect |
|---|---|
| `vibrant mural` | Urban, colorful background |
| `foggy street` | Atmospheric, moody setting |
| `futuristic city` | Sci-fi urban environment |
| `alien landscape with clear, azure water` | Otherworldly location |
| `forgotten forest` | Mysterious, overgrown nature |
| `contemplative` | Reflective, quiet mood |
| `grounded atmosphere` | Realistic, earthy tone |
| `faint city murmurs` | Urban ambient sound |

### Audio Vocabulary (Omni-Specific)

| Term | Usage |
|---|---|
| `mellow, soulful hip-hop beat` | Background music mood |
| `calm smooth music` | Relaxed audio atmosphere |
| `in sync with the music` | Audio-reactive visuals |
| `compelling voiceover` | Narration instruction |
| `city murmurs and distant chatter` | Ambient sound design |

---

## 9. Negative Prompting

Gemini Omni explicitly supports negative constraints. Verified working examples from official docs[^14]:

### Negative Prompt Patterns

```
Don't add seahorses.
No voice cuts at the end.
Don't add text.
no hands
```

### Negative Prompt Guidelines

- Use **natural language** (not special syntax): `"Don't add..."` · `"No..."` · `"no [element]"`
- Can cluster **multiple negatives** in one prompt (up to 3+ verified)
- Place negative constraints **at the end** of the prompt for clarity
- Effective for: removing unwanted objects, preventing text overlays, controlling audio behavior, specifying missing body parts

---

## 10. Veo 3 Prompt Guide (Comparison / When Veo is Used)

Veo 3.1 is Google DeepMind's standalone video generation model. Use when you need **maximum cinematic precision** and don't need conversational editing[^15].

### Veo vs. Omni: When to Use Which

| Need | Use |
|---|---|
| Iterative video editing | **Gemini Omni** |
| Multi-modal references (`<video>`, `<image>`, `<audio>`) | **Gemini Omni** |
| Avatar / character references (`@me`) | **Gemini Omni** |
| 10-second videos | **Gemini Omni** |
| Maximum cinematic quality, single generation | **Veo 3.1** |
| First + last frame control | **Veo 3.1** |
| 4K resolution | **Veo 3.1** |
| API access | **Veo 3.1** (model: `veo-3.1-generate-preview`) |

### Veo Prompt Framework (7 Pillars)

| Pillar | Veo 3 Guideline |
|---|---|
| **Shot Framing & Motion** | Low-angle, panning, camera movements — be specific |
| **Style** | Cartoon, claymation, film noir, VHS, stop-motion |
| **Lighting** | Warm/even, spotlight, directional |
| **Character Descriptions** | Specific physical attributes: *"A woman in her twenties with wavy brown hair and light freckles"* |
| **Location** | *"A smoky jazz club at night"* beats *"a jazz club"* |
| **Action** | Play-by-play for every element in fast-paced scenes |
| **Dialogue** | Give exact lines or topic for Veo 3's native audio generation |

### Veo Required Elements

| Element | Description | Example |
|---|---|---|
| **Subject** | Object, person, animal, or scenery | `cityscape` · `puppies` · `vehicles` |
| **Action** | What the subject is doing | `walking` · `turning their head` |
| **Style** | Creative direction / film style | `sci-fi` · `film noir` · `cartoon` |

### Veo Optional Elements

| Element | Examples |
|---|---|
| **Camera Position & Motion** | `aerial view` · `eye-level` · `dolly shot` · `worms eye` |
| **Composition** | `wide shot` · `close-up` · `single-shot` · `two-shot` |
| **Focus & Lens Effects** | `shallow focus` · `macro lens` · `wide-angle lens` |
| **Ambiance** | `blue tones` · `night` · `warm tones` · `natural light` · `sunrise` |

### Veo Audio Prompt Patterns

| Audio Type | Technique | Example |
|---|---|---|
| **Dialogue** | Use quotes | `"This must be it," he murmured.` |
| **Sound Effects** | Explicit description | `tires screeching loudly, engine roaring` |
| **Ambient** | Describe soundscape | `A faint, eerie hum resonates in the background` |

### Veo Example Prompts

**Character + Dialogue + Environment:**
```
A medium shot frames an old sailor, his knitted blue sailor hat casting a shadow
over his eyes, a thick grey beard obscuring his chin. He holds his pipe in one
hand, gesturing with it towards the churning, grey sea beyond the ship's railing.
"This ocean, it's a force, a wild, untamed might. And she commands your awe,
with every breaking light"
```

**World Building / Atmosphere:**
```
A snow-covered plain of iridescent moon-dust under twilight skies. Thirty-foot
crystalline flowers bloom, refracting light into slow-moving rainbows. A
fur-cloaked figure walks between these colossal blossoms, leaving the only
footprints in untouched dust.
```

**Simple vs. Detailed — Snow Leopard:**

Simple:
```
A cute creature with snow leopard-like fur is walking in winter forest, 3D cartoon style render.
```

Detailed:
```
Create a short 3D animated scene in a joyful cartoon style. A cute creature with
snow leopard-like fur, large expressive eyes, and a friendly, rounded form happily
prances through a whimsical winter forest. The scene should feature rounded,
snow-covered trees, gentle falling snowflakes, and warm sunlight filtering through
the branches. The creature's bouncy movements and wide smile should convey pure
delight. Aim for an upbeat, heartwarming tone with bright, cheerful colors and
playful animation.
```

**Fully Annotated Icicles Prompt (element labels):**
```
Close up shot [composition] of melting icicles [subject] on a frozen rock wall
[context] with cool blue tones [ambiance], zoomed in [camera motion] maintaining
close-up detail of water drips [action].
```

### Veo API Parameters

| Parameter | Veo 3.1 | Notes |
|---|---|---|
| `model` | `veo-3.1-generate-preview` | Also: `veo-3.1-fast-generate-preview`, `veo-3.1-lite-generate-preview` |
| `aspectRatio` | `"16:9"` (default), `"9:16"` | Portrait available |
| `durationSeconds` | `"4"`, `"6"`, `"8"` | Must be 8s for extension / ref images / 1080p / 4K |
| `resolution` | `720p` (default), `1080p`, `4k` | 1080p/4K require 8s duration |

---

## 11. Gemini Image Generation — Nano Banana Prompt Guide

**Nano Banana** is the brand name for Gemini's native image generation[^16]:

| Model | API ID | Best For |
|---|---|---|
| Nano Banana 2 | `gemini-3.1-flash-image` | High-efficiency, high-volume, low-latency |
| Nano Banana Pro | `gemini-3-pro-image` | Professional assets, advanced reasoning |
| Nano Banana | `gemini-2.5-flash-image` | Speed & efficiency for high-volume |

### Core Principle

> **"Describe the scene, don't just list keywords."** The model's primary strength is its deep language understanding. A descriptive, narrative paragraph will almost always produce a better, more coherent image than a list of disconnected words.

### 7 Image Prompt Strategies

#### 1. Photography
Use photographic terminology: camera angles, lens types, lighting.

```
A close-up portrait photo of an elderly Japanese ceramicist with deep sun-etched
wrinkles and a warm, knowing smile. He is carefully examining a freshly glazed
tea bowl. The setting is his rustic, sun-warmed studio. The scene is lit by soft
golden-hour light entering from a window, highlighting fine clay texture. Shot
with an 85mm portrait lens, producing a soft, blurred background (bokeh). The
overall atmosphere is serene and masterful. Vertical orientation.
```

#### 2. Stylized Illustrations & Stickers
Specify the style and request a white background for clean cutouts.

```
A kawaii-style sticker of a happy red panda wearing a tiny bamboo hat. It is
eating a green bamboo leaf. The design features thick, clean outlines, simple
cel-shading, and a vibrant color palette. The background should be white.
```

#### 3. Accurate Text in Images
Gemini excels at text rendering — describe content, font style, and overall design.

```
Create a modern, minimalist logo for a coffee shop called 'The Daily Grind'. The
text should be in a clean, bold sans-serif font. The color scheme is black and
white. Place the logo inside a circle. Incorporate a coffee bean cleverly.
```

#### 4. Product Mockups & Commercial Photography

```
High-resolution studio-lit product photography of a minimalist matte black ceramic
coffee mug, presented on a polished concrete surface. The lighting is a three-point
softbox setup designed to create soft, diffuse highlights and eliminate harsh
shadows. The camera angle is a slightly elevated 45-degree shot to show clean lines.
Ultra-realistic, with sharp focus on steam rising from coffee. Square image.
```

#### 5. Minimalist & Negative Space Design

```
A minimalist composition with a single, delicate red maple leaf positioned in the
lower-right corner of the frame. The background is a vast, empty off-white canvas
creating significant negative space for text. Soft, diffused lighting from the
upper left. Square image.
```

#### 6. Sequential Art (Comic Panel / Storyboard)

```
Create a three-panel comic strip in a raw noir art style with high-contrast black
and white inks. Place the character in a humorous scene.
```
*(Best with Gemini 3.1 Pro/Flash Image + input image of character)*

#### 7. Grounding with Google Search
For real-time or recent events:

```
Use image search to find an accurate image of the beautiful quetzal bird.
Create a beautiful 3:2 wallpaper of this bird, with a natural top-to-bottom
gradient and a minimal composition.
```

### Image Editing Prompts

**Add element (matching style):**
```
Using the provided image of my cat, add a small knitted wizard hat on his head.
Make it look like it's resting comfortably and fitting with the soft lighting of the photo.
```

**Multi-turn infographic:**
```
Create a vibrant infographic that explains photosynthesis as if it were a recipe
for a plant's favorite food. Show the 'ingredients' (sunlight, water, CO2) and
the 'finished dish' (sugar/energy). The style should be like a page from a
colorful kids' cookbook, suitable for a 4th grader.
```

Then iterate:
```
Update this infographic to be in Spanish. Do not change any other elements of the image.
```

### Aspect Ratios & Resolution

**Supported aspect ratios:** `1:1` · `1:4` · `1:8` · `2:3` · `3:2` · `3:4` · `4:1` · `4:3` · `4:5` · `5:4` · `8:1` · `9:16` · `16:9` · `21:9`

**Supported resolutions:** `512` (Flash only) · `1K` · `2K` · `4K`

> ⚠️ Must use **UPPERCASE 'K'** (e.g., `1K`, `2K`, `4K`). Lowercase will be rejected.

---

## 12. Audio Generation — TTS Prompt Guide

Gemini's TTS is powered by `gemini-3.1-flash-tts-preview` and `gemini-2.5-pro-preview-tts`[^17].

> *"Gemini Native Audio Generation TTS differs from traditional TTS because it uses a large language model that knows not only what to say, but also how to say it."*

### Available Voices (30 Total)

| Voice | Style | Voice | Style |
|---|---|---|---|
| **Zephyr** | Bright | **Aoede** | Breezy |
| **Puck** | Upbeat | **Callirrhoe** | Calm |
| **Charon** | Informational | **Autonoe** | Bright |
| **Kore** | Firm | **Enceladus** | Breathy |
| **Fenrir** | Excitable | **Iapetus** | Clear |
| **Leda** | Youthful | **Umbriel** | Casual |
| **Orus** | Firm (Lawyer-like) | **Algieba** | Smooth |
| **Despina** | Smooth | **Rasalgethi** | Informative |
| **Erinome** | Easygoing | **Laomedeia** | Upbeat |
| **Algenib** | Gravelly | **Achernar** | Soft |
| **Alnilam** | Firm | **Schedar** | Even |
| **Gacrux** | Mature | **Pulcherrima** | Frank |
| **Achird** | Friendly | **Zubenelgenubi** | + 5 more |

**Voice selection tip:** Match voice to emotion: `Enceladus` (breathy) for tired/bored; `Puck` (upbeat) for excited.

---

### Audio Tags — Expressive Inline Controls

Insert `[tag]` directly in transcript text to control specific moments:

**Emotion tags:**
```
[amazed]  [crying]  [curious]  [excited]  [sighs]  [gasp]
[giggles]  [laughs]  [mischievously]  [panicked]  [sarcastic]
[serious]  [shouting]  [tired]  [trembling]  [whispers]
```

**Style/Pacing tags:**
```
[excitedly]  [bored]  [reluctantly]
[very fast]  [very slow]
[sarcastically, one painfully slow word at a time]
```

**Character/Creative tags:**
```
[like a cartoon dog]  [like Dracula]  [cough]  [yawn]
```

### Audio Tag Examples

**Whisper + Shout combo:**
```
[whispers] Hi, I'm a new text-to-speech model [shouting] and I can say things
in many different ways. [whispers] How can I help you today?
```

**Multi-speaker with emotion direction:**
```
Make Speaker1 sound tired and bored, and Speaker2 sound excited and happy:

Speaker1: So... [yawn] what's on the agenda today?
Speaker2: You're never going to guess!
```

**Spooky pause + whisper:**
```
Say in a spooky voice:
"By the pricking of my thumbs... [short pause]
[whisper] Something wicked this way comes"
```

---

### Advanced TTS Prompt Framework

Structure a TTS prompt like a **voice acting direction sheet**:

```
# AUDIO PROFILE: [Character Name]
## "[Episode/Scene Title]"

## THE SCENE: [Setting Name]
[Physical environment + mood description]

### DIRECTOR'S NOTES
Style: [Performance notes — emotion, energy, approach]
Pace: [Speed and rhythm guidance]
Accent: [Specific regional accent]

### SAMPLE CONTEXT
[Brief archetype description for model alignment]

#### TRANSCRIPT
[Exact words with inline [audio tags] for specific moments]
```

**Full Example:**
```
# AUDIO PROFILE: Jaz R.
## "The Morning Hype"

## THE SCENE: The London Studio
It is 10:00 PM in a glass-walled studio overlooking the moonlit London skyline.
The red "ON AIR" tally light is blazing. Jaz is bouncing on the balls of their
heels to a thumping backing track.

### DIRECTOR'S NOTES
Style:
* The "Vocal Smile": You must hear the grin in the audio.
* Dynamics: High projection without shouting. Punchy consonants and elongated
  vowels on excitement words (e.g., "Beauuutiful morning").
Pace: Energetic, fast, bouncing cadence. No dead air.
Accent: Jaz is from Brixton, London

#### TRANSCRIPT
[excitedly] Yes, massive vibes in the studio! You are locked in and it is
absolutely popping off in London right now. [shouting] Turn this up!
We've got the project roadmap landing in three, two... let's go!
```

---

## 13. Audio Understanding Prompting

For analyzing/transcribing audio (not generating)[^18]:

### Capabilities
- Describe, summarize, Q&A on audio content
- Transcription with timestamps, language detection, translation
- Emotion detection in speech and music
- Timestamp-specific segment extraction

### Prompt Templates

**Full transcription with timestamps + emotion:**
```
Process the audio file and generate a detailed transcription.
Requirements:
1. Provide accurate timestamps for each segment (Format: MM:SS).
2. Detect the primary language of each segment.
3. If the segment is in a language different than English, provide the English translation.
4. Identify the primary emotion: MUST choose exactly one of: Happy, Sad, Angry, Neutral.
5. Provide a brief summary of the entire audio at the beginning.
```

**Timestamp-specific extraction:**
```
Provide a transcript of the speech from 02:30 to 03:29.
```

**Simple transcription:**
```
Generate a transcript of the speech.
```

### Technical Specs

| Property | Value |
|---|---|
| Token rate | 32 tokens per second |
| 1 minute of audio | 1,920 tokens |
| Max audio duration | 9.5 hours per prompt |
| Sampling rate | 16 Kbps (downsampled) |
| Supported formats | WAV · MP3 · AIFF · AAC · OGG · FLAC |

---

## 14. Text Prompting Best Practices (Gemini API)

These guidelines apply to all Gemini text/multimodal prompts via the API[^19]:

### 14.1 Prompt Input Types

| Type | Description | Example |
|---|---|---|
| **Question** | Model answers | *"What's a good name for a dried flower shop? Give 5 options."* |
| **Task** | Model performs action | *"Give me a simple list of 5 camping essentials."* |
| **Entity** | Model classifies | *"Classify: Elephant, Mouse, Snail as [large, small]."* |
| **Partial Completion** | Model extends partial input | Provide JSON prefix → model fills in object |

### 14.2 Few-Shot Prompting

- **Zero-shot:** No examples — works but less reliable
- **Few-shot (recommended):** Provide 2–5 examples
- **Consistent formatting** across all examples: same XML tags, whitespace, splitters
- **Warning:** Too many examples → overfitting

```
Q: [Question]
A: [Answer]

Q: [Question]
A: [Answer]

Q: [New question]
A:
```

### 14.3 Formatting & Constraints

- Tell the model **what to do AND what NOT to do**
- Specify exact output format: Markdown, JSON, HTML, table, bullet list
- Use **completion strategy** — start the format, let the model continue

### 14.4 XML-Structured Prompt Template

```xml
<role>
You are a helpful assistant.
</role>

<constraints>
1. Be objective.
2. Cite sources.
</constraints>

<context>
[Insert User Input Here]
</context>

<task>
[Insert the specific user request here]
</task>
```

### 14.5 Markdown-Structured Prompt Template

```markdown
# Identity
You are a senior solution architect.

# Constraints
- No external libraries allowed.
- Python 3.11+ syntax only.

# Output format
Return a single code block.
```

### 14.6 Full System Instruction Template

```xml
<!-- SYSTEM INSTRUCTION -->
<role>
You are Gemini 3, a specialized assistant for [Domain].
You are precise, analytical, and persistent.
</role>

<instructions>
1. **Plan**: Analyze the task and create a step-by-step plan.
2. **Execute**: Carry out the plan.
3. **Validate**: Review your output against the user's task.
4. **Format**: Present the final answer in the requested structure.
</instructions>

<constraints>
- Verbosity: [Low/Medium/High]
- Tone: [Formal/Casual/Technical]
</constraints>

<output_format>
1. **Executive Summary**: [Short overview]
2. **Detailed Response**: [Main content]
</output_format>

<!-- USER PROMPT -->
<context>
[Insert context/documents/code here]
</context>

<task>
[Insert specific request here]
</task>

<final_instruction>
Remember to think step-by-step before answering.
</final_instruction>
```

### 14.7 Gemini 3 / Advanced Model Principles

- **Be precise and direct** — avoid unnecessary persuasive language
- **Use consistent structure** — XML tags or Markdown headings (pick one)
- **Define ambiguous terms explicitly**
- **Critical instructions first** — role definitions, behavioral constraints, format requirements go in system instructions or at very start
- **Long context:** Supply all context FIRST, then questions at the **very end**
- **Anchor transition:** After a large data block, use: *"Based on the information above..."*
- **Treat all modalities equally** — reference each modality clearly when mixing text + image + audio

### 14.8 Context Injection

```
Answer the question using the text below. Respond with only the text provided.

[Context text here]

Question: [Your question]
```

### 14.9 Prompt Decomposition Strategies

1. **Break down instructions** — one prompt per instruction type
2. **Prompt chaining** — output of prompt N becomes input to prompt N+1
3. **Aggregate responses** — parallel tasks on data subsets, then merge

### 14.10 Iteration Strategies

1. **Rephrase** — different wording often yields different results
2. **Switch task type** — reformulate as question vs. task vs. completion
3. **Change content order** — try `[examples][context][input]` vs `[input][examples][context]`
4. **Adjust temperature** — if hitting safety filters, try increasing temperature

### 14.11 Image Prompting Best Practices

- ✅ Place **image BEFORE text** in single-image prompts
- ✅ Use **clear, non-blurry images**
- ✅ Verify images are **correctly rotated**
- ✅ For > 20 MB files: use Files API

**Troubleshooting multimodal prompts:**

| Problem | Solution |
|---|---|
| Model not using relevant image area | Drop hints about which aspects to reference |
| Output too generic | Ask model to describe the image first, then the task |
| Need to debug | Ask: *"Describe this image"* then *"Explain your reasoning"* |
| Hallucinated content | Lower temperature; ask for shorter descriptions |
| Wrong output format | Add few-shot examples |

---

## 15. Thinking Mode Reference

### Gemini 3 Series — `thinkingLevel`

| Level | Best For |
|---|---|
| `minimal` | Near-off; lowest latency; simple queries |
| `low` | Simple instructions, chat, high-throughput apps |
| `medium` | Default for 3.5 Flash; most balanced tasks |
| `high` | Default for 3.1 Pro; complex math, coding, research |

### Gemini 2.5 Series — `thinkingBudget` (Token Count)

| Model | Default | Range | Disable |
|---|---|---|---|
| 2.5 Pro | Dynamic | 128–32,768 | Cannot disable |
| 2.5 Flash | Dynamic | 0–24,576 | `thinkingBudget=0` |
| 2.5 Flash Lite | Off | 512–24,576 | `thinkingBudget=0` |

### Task Complexity Guide

| Complexity | Recommendation | Example Tasks |
|---|---|---|
| **Easy** | Disable thinking | *"Where was DeepMind founded?"* |
| **Medium** | Default / some thinking | *"Compare EVs vs hybrids"* |
| **Hard** | Maximum thinking | AIME math problems / Full-stack app design |

**Prompting tip:** *"Think very hard before answering"* improves performance on hard reasoning tasks (at cost of extra tokens).

**Debugging tip:** If model is wrong, enable `include_thoughts=True` to read the reasoning chain and identify where it diverged.

---

## 16. API Parameters Quick Reference

### Gemini Text/Multimodal Models

| Parameter | Effect |
|---|---|
| `temperature` | `0` = deterministic · Higher = more creative/diverse |
| `topK` | Limits token selection pool (`topK=1` = greedy) |
| `topP` | Cumulative probability threshold (default: `0.95`) |
| `max_output_tokens` | ~4 chars per token; 100 tokens ≈ 60–80 words |
| `stop_sequences` | Characters that halt generation |

### Gemini Image Models (Nano Banana)

| Parameter | Notes |
|---|---|
| `aspect_ratio` | `"1:1"` · `"16:9"` · `"9:16"` · `"3:2"` etc. |
| `resolution` | `"512"` · `"1K"` · `"2K"` · `"4K"` (UPPERCASE K required) |
| `thinking_level` | `"minimal"` · `"high"` (Flash Image only) |
| Reference images | Up to 10 object + 4 character refs (Flash); up to 6+5 (Pro) |

### Veo Video Models

| Parameter | Notes |
|---|---|
| `model` | `veo-3.1-generate-preview` (recommended) |
| `aspect_ratio` | `"16:9"` (default) · `"9:16"` (portrait) |
| `duration_seconds` | `"4"` · `"6"` · `"8"` |
| `resolution` | `720p` · `1080p` · `4k` (requires 8s duration) |

---

## 17. Safety, Watermarking & Access

### Safety Mechanisms

| Mechanism | Description |
|---|---|
| **Continuous Evaluation** | Training + automated human evals throughout lifecycle |
| **Human Red Teaming** | Specialist teams external to model development |
| **Automated Red Teaming** | Dynamic scale evaluation of Gemini Omni Flash |
| **Ethics & Safety Reviews** | Pre-release review required |

### Content Provenance

| System | Description |
|---|---|
| **SynthID** | Imperceptible digital watermark on ALL AI-generated/edited content |
| **C2PA Content Credentials** | Industry-standard content provenance metadata — verifiable via Gemini app; coming to Chrome and Search |

### Access Requirements

- Gemini App and Google Flow: **Google AI subscription required**
- Features vary by tier and geography
- Developer API (`veo-3.1-generate-preview`, etc.): Gemini API key + billing

### Credit Costs (Google Flow)

| Generation Type | Credits |
|---|---|
| Omni Flash 4s video | 15 |
| Omni Flash 6s video | 20 |
| Omni Flash 8s video | 25 |
| Omni Flash 10s video | 30 |
| **Omni Flash video edit** | **40** (flat, any length) |
| Veo 3.1 Lite (any length) | 10 |
| Veo 3.1 Quality 8s | 100 |

---

## 18. Quick Prompt Templates

### Template A: Gemini Omni Video Generation (Full)

```
[SHOT TYPE, e.g., "A medium shot"] [CHARACTER/SUBJECT description].
[ACTION — what they are doing]. Set in [LOCATION with atmosphere].
Lit by [LIGHT SOURCE] creating a [LIGHT QUALITY] effect.
Style: [AESTHETIC DIRECTION].
[AUDIO: MUSIC/VOICEOVER/SOUND EFFECTS if needed].
[NEGATIVE CONSTRAINTS if needed: "Don't add...", "No..."].
```

**Example filled-in:**
```
A medium shot of a woman in her 30s with long dark hair standing at the edge
of a cliff, arms outstretched, facing the wind. Set in an alien landscape with
clear, azure water below. Lit by a double sunset creating an ethereal purple glow.
Style: cinematic, majestic, dreamlike. Calm orchestral score swells in the background.
Don't add text overlays.
```

---

### Template B: Gemini Omni Iterative Edit Chain

```
[Start] Generate: [base scene description]
[Edit 1] [Simple edit command, e.g., "Make the sky orange"]
[Edit 2] [Another edit, e.g., "Add falling cherry blossoms"]
[Edit 3] [Refinement, e.g., "Slow the camera movement down"]
```

---

### Template C: Ingredients Workflow (Google Flow)

```
[Upload 2-3 reference images as ingredients]
Prompt: "The [ingredient 1] [relationship/action] [ingredient 2] through [ingredient 3]."
```

---

### Template D: Multi-Modal Synthesis

```
The [element] from <video> [action/relationship] based on <image>.
They [respond/move/transform] to the [music/sound] from <audio> and [final outcome].
```

---

### Template E: Text-Animated Video

```
[Timing instruction: "word by word" / "one word at a time" / "letter by letter"],
[exact text to display: "word1, word2, word3..."].
[Animation instruction: "each word appears with a different animated style" / "flash"].
[Pacing: "perfect pacing to a rhythm" / "rapid fire" / "slow reveal"].
[Genre: "sizzle reel" / "title card" / "lyric video"].
```

---

### Template F: Educational/Explainer Video

```
Explain [CONCEPT]. 
Visualize this using [ART STYLE, e.g., "contemporary flat-media" / "claymation" / "stop motion"].
Color palette: [COLOR DESCRIPTION, e.g., "neon pinks, cyans on deep navy"].
Aesthetic details: [TEXTURE/FEEL, e.g., "stipple shading, grainy gradients, risograph-like quality"].
[NEGATIVE: "Don't add text" / "no hands"].
Accurate representation required.
```

---

### Template G: Audio TTS — Simple

```
[Persona direction]: [exact transcript with [tags] inline]

Example:
Excited radio host from London: [excitedly] Welcome back, you are listening 
to the best show in the city! [shouting] Turn it up!
```

---

### Template H: Veo Cinematic (Full)

```
[SHOT TYPE + CAMERA MOVEMENT]. [CHARACTER description with specific physical details].
[ACTION with emotional context]. Set in [SPECIFIC LOCATION with atmosphere detail].
[SOUND DESIGN: ambient + music + dialogue].
Dialogue: "[CHARACTER]: '[Line of dialogue.]'"
Style: [FILM GENRE/AESTHETIC].
```

---

## Confidence Assessment

| Claim | Confidence | Source |
|---|---|---|
| Five Core Prompt Elements | ✅ High — verbatim from official page | deepmind.google/models/gemini-omni/prompt-guide/ |
| Omni vs. Veo philosophy | ✅ High — direct quote | deepmind.google/models/gemini-omni/prompt-guide/ |
| All official prompt examples | ✅ High — scraped verbatim | deepmind.google/models/gemini-omni/ |
| Ingredients workflow + @syntax | ✅ High — from Google Flow support docs | support.google.com/flow/ |
| Veo 3 prompt guide | ✅ High — from dedicated Veo prompt guide page | deepmind.google/models/veo/prompt-guide/ |
| Nano Banana image guide | ✅ High — from official API docs | ai.google.dev/gemini-api/docs/image-generation |
| TTS voices and audio tags | ✅ High — from official API docs | ai.google.dev/gemini-api/docs/speech-generation |
| Voice list (30 total) | ⚠️ Partial — last ~5 voices not captured due to rendering truncation | ai.google.dev/gemini-api/docs/speech-generation |
| Text prompting best practices | ✅ High — from official API docs | ai.google.dev/gemini-api/docs/prompting-strategies |
| Thinking mode parameters | ✅ High — from official API docs | ai.google.dev/gemini-api/docs/thinking |
| Credit costs | ✅ High — from Google Flow support | support.google.com/flow/answer/16526234 |

---

## Footnotes

[^1]: [deepmind.google/models/gemini-omni/](https://deepmind.google/models/gemini-omni/) — Gemini Omni model page
[^2]: [deepmind.google/models/gemini-omni/prompt-guide/](https://deepmind.google/models/gemini-omni/prompt-guide/) — Official Gemini Omni Prompt Guide
[^3]: [deepmind.google/models/gemini-omni/prompt-guide/](https://deepmind.google/models/gemini-omni/prompt-guide/) — World Understanding section
[^4]: [deepmind.google/models/gemini-omni/](https://deepmind.google/models/gemini-omni/) — Model page prompt examples
[^5]: [deepmind.google/models/gemini-omni/prompt-guide/](https://deepmind.google/models/gemini-omni/prompt-guide/) — Prompt guide examples
[^6]: [support.google.com/flow/answer/16353334](https://support.google.com/flow/answer/16353334) — Google Flow: Create Videos
[^7]: [support.google.com/flow/answer/16352836](https://support.google.com/flow/answer/16352836) — Google Flow: Models & Features
[^8]: [support.google.com/flow/answer/17093911](https://support.google.com/flow/answer/17093911) — Google Flow: The Agent
[^9]: [deepmind.google/models/gemini-omni/prompt-guide/](https://deepmind.google/models/gemini-omni/prompt-guide/) — Closing tip
[^10]: [deepmind.google/models/gemini-omni/prompt-guide/](https://deepmind.google/models/gemini-omni/prompt-guide/) — Reference Complex Actions section
[^11]: [deepmind.google/models/gemini-omni/prompt-guide/](https://deepmind.google/models/gemini-omni/prompt-guide/) — Text Rendering section
[^12]: [deepmind.google/models/gemini-omni/](https://deepmind.google/models/gemini-omni/) — Multi-modal input prompt examples
[^13]: Synthesized from all official prompt examples and vocabulary across deepmind.google and ai.google.dev documentation
[^14]: [deepmind.google/models/gemini-omni/](https://deepmind.google/models/gemini-omni/) — Official prompt examples with explicit negatives
[^15]: [deepmind.google/models/veo/prompt-guide/](https://deepmind.google/models/veo/prompt-guide/) — Veo 3 Prompt Guide; [ai.google.dev/gemini-api/docs/video](https://ai.google.dev/gemini-api/docs/video) — Veo API docs
[^16]: [ai.google.dev/gemini-api/docs/image-generation](https://ai.google.dev/gemini-api/docs/image-generation) — Nano Banana Prompt Guide
[^17]: [ai.google.dev/gemini-api/docs/speech-generation](https://ai.google.dev/gemini-api/docs/speech-generation) — TTS Prompt Guide
[^18]: [ai.google.dev/gemini-api/docs/audio](https://ai.google.dev/gemini-api/docs/audio) — Audio Understanding
[^19]: [ai.google.dev/gemini-api/docs/prompting-strategies](https://ai.google.dev/gemini-api/docs/prompting-strategies) — Text Prompting Strategies
