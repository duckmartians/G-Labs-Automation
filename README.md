<h1 align="center">G-Labs Automation</h1>
<p align="center"><b>AI image & video generation automation for creators</b><br>
Batch-generate with Google Flow (Veo 3.1, Nano Banana Pro), Grok Imagine, Meta AI and ChatGPT GPT Image 2 — from one desktop app.</p>

<p align="center">
  <a href="https://github.com/duckmartians/G-Labs-Automation/releases/latest">
    <img alt="Download G-Labs Automation for Windows" src="https://img.shields.io/badge/Download%20Windows-%F0%9F%92%BB-0078D6?style=for-the-badge&logo=windows&logoColor=white">
  </a>
  <a href="https://github.com/duckmartians/G-Labs-Automation/releases/latest">
    <img alt="Download G-Labs Automation for macOS" src="https://img.shields.io/badge/Download%20macOS-%F0%9F%8D%8E-000000?style=for-the-badge&logo=apple&logoColor=white">
  </a>
</p>

<p align="center">
  <a href="https://duckmartians.info/g-labs">
    <img alt="G-Labs Automation homepage" src="https://img.shields.io/badge/Homepage-Visit-0A66C2?style=flat-square&logo=google-chrome&logoColor=white">
  </a>
  <a href="https://duckmartians.info/g-labs/guide/?lang=en">
    <img alt="G-Labs Automation user guide and documentation" src="https://img.shields.io/badge/Guide-Read-2ea44f?style=flat-square&logo=readthedocs&logoColor=white">
  </a>
  <a href="https://www.youtube.com/playlist?list=PLGHIReR0l_N0-c4wAJ518BNyEvsPnhCn_">
    <img alt="G-Labs Automation video tutorials on YouTube" src="https://img.shields.io/badge/Tutorials-Watch-FF0000?style=flat-square&logo=youtube&logoColor=white">
  </a>
</p>

<p align="center">
  <a href="https://discord.gg/munMZEBMw5">
    <img alt="Join the G-Labs Automation Discord community" src="https://img.shields.io/badge/dynamic/json?url=https://discord.com/api/guilds/1369302820037201981/widget.json&query=$.presence_count&label=Discord&color=5865F2&logo=discord&logoColor=white&style=flat-square">
  </a>
  <a href="https://t.me/+eXFkN4vi1Tg3Zjg9">
    <img alt="Join the G-Labs Automation Telegram group" src="https://img.shields.io/badge/Telegram-Join-26A5E4?style=flat-square&logo=telegram&logoColor=white">
  </a>
  <a href="https://zalo.me/g/fqkbwmwfpjnh32eg6hqv">
    <img alt="Join the G-Labs Automation Zalo group" src="https://img.shields.io/badge/Zalo-Join-0068FF?style=flat-square&logo=zalo&logoColor=white">
  </a>
</p>

<br>

## What is G-Labs Automation?

**G-Labs Automation** is a desktop app for **Windows** and **macOS** that turns AI image and video
generation into a repeatable batch job. Paste a list of prompts, pick a model, press Run — the app
rotates your accounts, paces the requests, retries what fails, and files every result into the right
folder. No scripting, no browser tabs, no command line.

Built for people who ship volume: content creators, ad-creative teams, faceless-channel operators,
print-on-demand sellers and agencies running AI production at scale.

---

## Features

| Workspace | What it does |
| --- | --- |
| **Flow Image** | Batch text-to-image and image-to-image on Google Labs Flow — Nano Banana Pro, Nano Banana 2, Nano Banana 2 Lite. Reference images, aspect ratios, upscaling. |
| **Flow Video** | Batch AI video on **Veo 3.1** **Omni Flash** — text-to-video, image-to-video, ingredients-to-video, and scene-chain sequences. |
| **Grok AI** | Grok Imagine in four modes: Text → Image, Image → Image, Text → Video, Image → Video. |
| **Meta AI** | Meta AI (Vibes) image and video generation with named reference slots for character, scene and style. |
| **GPT Image 2** | ChatGPT image generation over an OAuth login — quality, aspect ratio, reasoning effort and web search per task. |
| **Workflow** | Node graph editor: chain prompts, models and reference images into a reusable pipeline, then run the whole flow. |
| **Image Upscaler** | Offline GPU upscaling with the Real-ESRGAN engine — General, Anime, Digital Art, High Fidelity, Remacri and more. No credits, no upload. |
| **Video Editor** | Media pool, timeline, split, trim and export — assemble generated clips without leaving the app. |
| **Prompt Hub** | Built-in prompt library you can search, filter and push straight into any workspace. |
| **Characters** | Reference-image library that keeps a character, location or style consistent across every render. |
| **Webhook API** | Drive generation from your own tools and receive results programmatically. |

---

## Built for batch work

- **Multi-account rotation** — add several accounts per provider; the app round-robins them and skips
  any account that hits its quota, so a long run keeps going.
- **Per-account proxy** — assign a proxy to each account: HTTP, SOCKS4 or SOCKS5.
- **Queue manager** — group prompts into tasks, edit a task's settings after queueing, re-run failures,
  skip or delete whole tasks. Each task runs at its own thread count.
- **Threading with pacing** — configurable parallel threads plus a randomized delay between requests.
- **Retry and failover** — transient errors retry on a different account; policy blocks stop instead of
  burning quota.
- **Sessions** — save a working table and reload it later with prompts, references, settings and results intact.
- **Reference auto-match** — name your files `REF_CHR_DEAN.jpg` and every prompt mentioning that name
  picks the image up automatically. Exact-name or partial-keyword matching.
- **Bulk prompt import** from `.txt` or Excel (drag and drop), session export back to `.xlsx`, plus a
  detailed log for auditing a run.

---

## Install

1. Download the latest build for your platform from **[Releases](https://github.com/duckmartians/G-Labs-Automation/releases/latest)**.
2. **Windows** — run the installer. **macOS** — open the `.dmg` (separate builds for Apple Silicon and Intel).
3. Launch the app, open **Settings › Accounts**, and sign in to the providers you want to use.

Start on the free **Basic** plan. Upgrade inside **Settings › License Account** when you need more —
plans are purchased in-app.

> ⚠ Any purchase offered outside the app is a scam.

---

## Languages

The interface ships in 11 languages: **English, Tiếng Việt, 中文 (简体), Español, Português (Brasil),
Русский, हिन्दी, বাংলা, ไทย, Türkçe, اردو** — with light and dark themes.

---

## Links & community

| | |
| --- | --- |
| 🌐 **Homepage** | https://duckmartians.info/g-labs |
| 📘 **User guide** | [English](https://duckmartians.info/g-labs/guide/?lang=en) · [Tiếng Việt](https://duckmartians.info/g-labs/guide/?lang=vi) |
| 🎬 **Video tutorials** | [English playlist](https://www.youtube.com/playlist?list=PLGHIReR0l_N0-c4wAJ518BNyEvsPnhCn_) · [Playlist tiếng Việt](https://www.youtube.com/playlist?list=PLGHIReR0l_N2TbhPADNwn0aRbJfLf5YSj) |
| 💬 **Discord** | https://discord.gg/munMZEBMw5 |
| ✈️ **Telegram** | https://t.me/+eXFkN4vi1Tg3Zjg9 |
| 💙 **Zalo** | https://zalo.me/g/fqkbwmwfpjnh32eg6hqv |

<p align="center">
  <a href="https://duckmartians.info/g-labs">
    <img width="2380" height="4578" alt="G-Labs Automation — AI image and video generation automation app for Windows and macOS" src="https://github.com/user-attachments/assets/f63986e3-468b-48cf-8a1b-a23ce7a22010" />
  </a>
</p>

---

<details>
<summary><b>Keywords</b></summary>

ai automation tool · ai image generator · ai video generator · batch image generation · batch video
generation · google flow automation · google labs flow · veo 3 · veo 3.1 · nano banana pro · nano
banana 2 · grok imagine · grok ai image · grok ai video · meta ai vibes · chatgpt image generation ·
gpt image 2 · text to image · image to image · text to video · image to video · multi account
rotation · account round robin · proxy support · prompt queue · batch prompt runner · prompt library ·
character consistency · reference image matching · image upscaler · real-esrgan upscale · video editor ·
webhook api · content creation tool · faceless channel · print on demand · ai content pipeline ·
windows desktop app · macos desktop app · no-code ai tool

</details>

<p align="center"><sub>Built by <a href="https://duckmartians.info">duckmartians</a></sub></p>
