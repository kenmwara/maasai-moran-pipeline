# Maasai Moran — an autonomous AI filmmaking studio

> A system that **researches what goes viral, writes original films, shoots them with AI cameras that cannot exist, narrates them with one voice, scores their tension, posts them on schedule, and learns from every view** — with zero human touch per video.

🌐 **Channel:** [youtube.com/@MaasaiMoran-KK](https://www.youtube.com/@MaasaiMoran-KK) · 🏗️ **Stack:** Node.js, PM2, Veo 3 (Kie), Claude, ElevenLabs, gpt-image-1, ffmpeg, YouTube Data API v3 · 🔒 **Source:** private — this is the public write-up

---

## 🧠 The YouTube Brain

<p align="center">
  <img src="assets/youtube-brain.png" alt="The YouTube Brain — a glowing neural map of viral research flowing into video creation" width="820">
</p>

Before a single frame renders, **The Brain** decides what deserves to exist. It is a standalone research engine that:

- **Harvests viral outliers** across the niche (keyless — search, velocity, transcripts of the hooks that worked)
- **Extracts patterns with evidence** — every pattern cites the videos that prove it; no vibes-based strategy
- **Generates channel identities in three bands** (close to the niche / adjacent / same frameworks, different world), scores them on viral ceiling × fit × speed-to-monetize, and locks the winner as a **fingerprint**
- **Mints video ideas** from the active fingerprint — each with a distinct narrative device, emotional core, and a wow-test ("would a jaded editor screenshot this title?")
- **Gates every generation**: before any Short or film is written, the full posted history (title, device, theme, format) is loaded and the new concept is *verified different* — a device-clash triggers regeneration. Diversity drives virality, structurally.
- **Learns from reality**: an analytics loop matches published videos back to the patterns that spawned them — winners strengthen their patterns, flops decay them, and the next research cycle thinks with the new weights.

The Brain's provenance graph (viral sources → patterns → identities → fingerprint → ideas → published videos) renders as a live force-directed **Brain Map** — glowing, type-colored, with flow particles feeding the video-creation hub. The image above is its likeness.

## 🎬 The production engine (v2)

Every video is manufactured, not generated:

```
The Brain (gate: device/theme vs full posted history)
        │
        ▼
Claude screenwriter ── theme · stakes · education · nested-loop retention grammar
        │               first-1.5s hook · tension moves (build/hold/release) per scene
        ▼
Frame-chained Veo 3 scenes ── each scene renders FROM the previous scene's final
        │                     frame (image-to-video): character, wardrobe and light
        │                     are physically continuous, not prompt-hopefully
        ▼
One narrator, post-produced ── every clip is speech-free; a single fixed TTS voice
        │                      is mixed over tension-scored ambience per scene
        ▼
Crossfaded assembly ── 0.35s video+audio dissolves at every join; seamless loop
        │              anchor for Shorts (last frame = first frame)
        ▼
YouTube Data API v3 ── direct resumable upload · AI-content disclosure · custom
        │              gpt-image-1 thumbnail · tags · auto-posted engagement comment
        ▼
Telegram ops ping ─── then a detached analytics chain (2h/24h/72h) feeds the
                      performance model without ever blocking the next slot
```

**Formats:** ~64-second Shorts (8 chained acts, three-act structure with room to land) twice daily, and ~3-minute original 16:9 films twice weekly — every film a **new narrative device the channel has never used** (the Brain's history feedback makes formats structurally unrepeatable).

## 🛡️ Built to run unattended

- **Slot scheduler** on absolute wall-clock (Shorts 16:00 & 23:00 UTC, films Wed + Sat 15:00 UTC) — restarts re-sleep to the same slot, no drift, no double-posts
- **Credit preflight** before every film — a run that can't finish never starts
- **Per-scene resilience** — safety-filter false positives get a sanitized rewrite, then a skip; a 23-scene film beats an aborted one
- **Screenplay persistence + resume** — an interrupted film re-renders only its missing scenes
- **Character persistence rules** — no cast member may vanish, morph, or be replaced by fire (learned the hard way)
- **Fail-soft everything** — a dead Telegram moment, a flaky image host, or a blocked scene never kills a cycle; failures ping the operator and the next slot proceeds
- Runs on a shared trading droplet under PM2 at `nice -15`/idle-IO with a hard memory cap — the studio can never starve the systems that pay for it

## 💸 Unit economics

| Asset | Cost | Cadence |
|---|---|---|
| 64s Short (8 × Veo 3 Fast clips + TTS + thumbnail) | ~$3.20 | 2× daily |
| ~3-min film (24 chained scenes + narration + thumbnail) | ~$9.60 | 2× weekly |
| **Studio total** | **~$65/week** | ~18 originals/week |

A one-person film studio's monthly output for the price of a dinner.

---

*Everything above runs autonomously today. The operator's job is taste: watch, judge, and tune the mandates — the machine does the rest.*
