# Maasai Moran — automated YouTube content pipeline

> End-to-end automation for a long-form storytelling channel: generation, posting, and analytics. Python + PM2 + custom dashboard.

🌐 **Channel:** [youtube.com/@maasaimoran](https://youtube.com/@maasaimoran) *(or similar)*
🏗️ **Stack:** Python, PM2, YouTube Data API, custom analytics dashboard (HTML/JS)
🔒 **Source:** private

---

## What this is

A content pipeline that takes a brief through generation → editing → posting → analytics for a YouTube storytelling channel. Built to reduce the per-video operator time from hours to minutes, with full analytics visibility on every upload.

## Pipeline

```
Brief (markdown)
       │
       ▼
generate_and_post.py  ─→  text generation → voice → b-roll → final cut
       │
       ▼
YouTube upload via Data API (scheduled or immediate)
       │
       ▼
Analytics fetcher (every hour)
       │
       ▼
maasai_youtube_analytics_dashboard.html
   (per-video views, retention, CTR, subscribers gained)
       │
       ▼
Events to ops.tbot.trade  (uploads, views, milestones)
```

## Architecture

- **Orchestration:** `generate_and_post.py` — main Python pipeline, modular per stage so steps can be re-run individually
- **Process supervision:** PM2 (`ecosystem.config.js`) — keeps the analytics fetcher alive, schedules generation runs, restarts on failure
- **Analytics:** custom HTML/JS dashboard backed by JSON files written by the analytics fetcher — no DB needed at current scale
- **Storage:** local filesystem for raw assets; published videos live on YouTube
- **Reporting:** every upload + milestone POSTs to `events.tbot.trade/ingest` for cross-portfolio visibility

## Key decisions

### 1. Generation as a script, not a SaaS

Comparable SaaS tools cost $50-300/mo and box you into their template library. A Python script + the right APIs is a one-time build that scales with usage rather than subscription tier.

### 2. PM2 for process supervision, not systemd or Docker

PM2 was already running for the trading bot loops; reusing it for the content pipeline meant zero new ops surface. Single `pm2 status` shows the whole portfolio's runtime state.

### 3. Custom analytics dashboard instead of YouTube Studio

YouTube Studio is great for one channel; it's terrible for cross-channel rollups or for piping data into the unified ops dashboard. A 200-line HTML/JS dashboard reading from JSON files gives me exactly the views I want and writes events into the broader portfolio analytics.

### 4. Modular pipeline stages

`generate_and_post.py` is structured so each stage (text → voice → b-roll → cut → upload) can be invoked individually. Re-running just one stage (e.g. swap voice models for an existing script) doesn't require regenerating everything upstream.

## What I'd build next

1. **A/B title testing** — generate 3 candidate titles per video, pick the best after the first 1000 views, update via YouTube API
2. **Cross-post automation** — short-form cuts to TikTok / Reels from the same source assets
3. **Sponsor-slot detection** — scan analytics to identify which videos earn best-retention sponsor breaks for upsell to brand deals

## Why this is interesting

It's a complete content business running as a Python script under PM2 alongside a trading bot. **One operator. Two unrelated revenue streams. Same supervision substrate.** The discipline transfers — what you learn building algorithmic trading systems (idempotent retries, structured logging, clean state machines) makes content pipelines vastly more reliable than the "no-code automation" alternatives.

---

*Source code is private. Architecture and decisions above are the showcase. To discuss, [reach out](https://linkedin.com/in/kenmwara).*
