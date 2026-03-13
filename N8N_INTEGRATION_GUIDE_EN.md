<p align="center">
  <a href="N8N_INTEGRATION_GUIDE_EN.md"><img src="https://img.shields.io/badge/English-blue" alt="English"></a>
  <a href="N8N_INTEGRATION_GUIDE_VI.md"><img src="https://img.shields.io/badge/Tiếng%20Việt-green" alt="Tiếng Việt"></a>
</p>

# 🔗 G-Labs Webhook × n8n — Step-by-Step Integration Guide

> **Automate AI image & video generation** by connecting G-Labs Automation with n8n — the open-source workflow automation platform.

> ⚠️ **Requirement:** **MAX** plan required for Webhook API.

---

## 📑 Table of Contents

| # | Section |
|---|---------|
| 1 | [Overview](#overview) |
| 2 | [Prerequisites](#prerequisites) |
| 3 | [Enable Webhook Server in G-Labs](#enable-webhook) |
| 4 | [Workflow 1: Generate AI Images](#workflow-image) |
| 5 | [Workflow 2: Generate AI Videos](#workflow-video) |
| 6 | [Workflow 3: Batch Generation](#workflow-batch) |
| 7 | [Error Handling & Advanced Tips](#error-handling) |
| 8 | [API Reference](#api-reference) |

---

<a name="overview"></a>

## 1. 🌟 Overview

### What is the G-Labs Webhook API?

G-Labs Automation provides a **local REST API server** running on your computer. You can send image/video generation requests from any tool — including **n8n**.

### Why use n8n?

- **Full automation**: Schedule image/video creation by time or events
- **Multi-platform integration**: Pull prompts from Google Sheets, Telegram, email → generate images → send results
- **Drag-and-drop interface**: No coding required
- **Free & self-hosted**: Runs on your own machine

### How it works

```
n8n Workflow → [HTTP Request] → G-Labs Webhook API (127.0.0.1:8765)
                                        ↓
                              Added to G-Labs queue
                                        ↓
                              G-Labs generates image/video
                                        ↓
n8n Workflow ← [Polling / Check status] ← Returns result (file URLs)
```

---

<a name="prerequisites"></a>

## 2. 🛠️ Prerequisites

### 2.1 Requirements

| Component | Details |
|-----------|---------|
| **G-Labs Automation** | Installed, **MAX** plan active |
| **n8n** | Installed ([installation guide](#install-n8n)) |
| **Google Account** | At least 1 valid account in G-Labs |

<a name="install-n8n"></a>

### 2.2 Install n8n (if not already installed)

**Option 1 — n8n Desktop (simplest):**

1. Visit [n8n.io/get-started](https://n8n.io/get-started)
2. Download the **Desktop** version for Windows
3. Install and open n8n → interface opens at `http://localhost:5678`

**Option 2 — npm (if you have Node.js):**

```bash
npm install n8n -g
n8n start
```

**Option 3 — Docker:**

```bash
docker run -it --rm --name n8n -p 5678:5678 n8nio/n8n
```

> 💡 **Important:** n8n and G-Labs **must run on the same computer** (the Webhook API only listens on `127.0.0.1`).

---

<a name="enable-webhook"></a>

## 3. 🚀 Enable Webhook Server in G-Labs

### Step 3.1 — Open the Webhook page

1. Open **G-Labs Automation**
2. In the left sidebar, click the **Webhook** icon (🔗)

### Step 3.2 — Get your API Key

1. On the Webhook page, you'll see the **API Key** field — this is your authentication key
2. Click **Copy** to copy the API Key
3. **Save it** — you'll need it when configuring n8n

> 💡 To generate a new key, click **Generate** (🔑). The old key will be invalidated.

### Step 3.3 — Configure Port

1. Default port: **8765**
2. If the port conflicts, enter a new port number (range: 1024–65535)

### Step 3.4 — Start the Server

1. Click **▶ Start Server**
2. Status changes to **🟢 Server Online**
3. Quick test: open your browser and visit `http://127.0.0.1:8765/api/health`

You should see a response like:

```json
{
  "status": "ok",
  "server": "G-Labs Webhook",
  "uptime": 10,
  "tasks_pending": 0,
  "tasks_running": 0
}
```

---

<a name="workflow-image"></a>

## 4. 🖼️ Workflow 1: Generate AI Images

> **Goal:** Send 1 prompt → G-Labs generates image → Retrieve results.

### 📋 Flow overview

```
[Manual Trigger] → [HTTP: Generate] → [Wait 30s] → [HTTP: Check Status] → [IF completed?]
                                                                               ↓ Yes
                                                                        [HTTP: Download]
                                                                               ↓ No
                                                                        [Back to Wait]
```

---

### Step 4.1 — Create a new Workflow

1. Open n8n (`http://localhost:5678`)
2. Click **+ Add workflow**
3. Name it: `G-Labs AI Image Generation`

---

### Step 4.2 — Add "Manual Trigger" node

1. A new workflow will have a **Manual Trigger** node by default
2. Keep it as-is

---

### Step 4.3 — Add "HTTP Request" node (Submit generation request)

1. Click **+** after Manual Trigger → select **HTTP Request**
2. Configure:

| Property | Value |
|----------|-------|
| **Method** | `POST` |
| **URL** | `http://127.0.0.1:8765/api/image/generate` |

3. Open **Headers** → click **Add Header**:

| Header Name | Header Value |
|------------|-------------|
| `X-API-Key` | `PASTE_YOUR_API_KEY` |

4. Open **Body** → select **JSON** → enter:

```json
{
  "prompt": "a beautiful sunset over the ocean, cinematic, 8k",
  "model": "imagen4",
  "count": 2,
  "aspect_ratio": "16:9",
  "upscale": ["2K"],
  "reference_images": []
}
```

> 💡 **Available models:** `imagen4`, `nano_banana`, `nano_banana_2`, `nano_banana_pro`

> 💡 **Available ratios vary by model** — see [API Reference](#api-reference) for details.

5. Rename the node: double-click the title → rename to `Generate Image`

---

### Step 4.4 — Add "Wait" node

1. Click **+** after "Generate Image" → search for **Wait**
2. Configure:

| Property | Value |
|----------|-------|
| **Resume** | `After Time Interval` |
| **Amount** | `30` |
| **Unit** | `Seconds` |

> 💡 Image generation typically takes **10–60 seconds**. 30 seconds is a good starting interval.

---

### Step 4.5 — Add "HTTP Request" node (Check status)

1. Click **+** after Wait → select **HTTP Request**
2. Configure:

| Property | Value |
|----------|-------|
| **Method** | `GET` |
| **URL** | `http://127.0.0.1:8765/api/status/{{ $('Generate Image').item.json.task_id }}` |

3. Add **Header** same as Step 4.3:

| Header Name | Header Value |
|------------|-------------|
| `X-API-Key` | `PASTE_YOUR_API_KEY` |

4. Rename the node: `Check Status`

> ⚠️ **Important:** The expression `{{ $('Generate Image').item.json.task_id }}` automatically extracts `task_id` from the "Generate Image" node's output. Make sure the node name matches exactly.

---

### Step 4.6 — Add "IF" node (Check completion)

1. Click **+** after "Check Status" → search for **IF**
2. Configure the condition:

| Property | Value |
|----------|-------|
| **Value 1** | `{{ $json.status }}` |
| **Operation** | `is equal to` |
| **Value 2** | `completed` |

3. Rename: `Is Completed?`

---

### Step 4.7 — Not done yet → Loop back to Wait

1. Drag the **false** output of "Is Completed?" → connect it **back** to the **Wait** node
2. This creates a loop: if not done, wait 30 seconds and check again

> 💡 n8n handles loops automatically. The workflow will keep polling until status = `completed` or `failed`.

---

### Step 4.8 — Completed → Download results

1. Drag the **true** output of "Is Completed?" → add a new **HTTP Request** node
2. Configure:

| Property | Value |
|----------|-------|
| **Method** | `GET` |
| **URL** | `{{ $('Check Status').item.json.results[0] }}` |

3. Open **Options** (to download as file):
   - **Response Format**: `File`
4. Rename: `Download Image`

> 💡 `results` is an array of URLs. `results[0]` is the first image, `results[1]` is the second, etc.

---

### Step 4.9 — Run it!

1. Make sure G-Labs is running + Webhook Server **🟢 ON**
2. Click **Test workflow** (▶️) in n8n
3. Watch each node light up as it executes
4. The final result will be the downloaded image file

---

<a name="workflow-video"></a>

## 5. 🎬 Workflow 2: Generate AI Videos

> Similar to the image workflow — just change the endpoint and parameters.

### Step 5.1 — Create a new Workflow

Create workflow `G-Labs AI Video Generation`, or duplicate the image workflow and edit.

### Step 5.2 — "HTTP Request" node (Generate video)

| Property | Value |
|----------|-------|
| **Method** | `POST` |
| **URL** | `http://127.0.0.1:8765/api/video/generate` |

**Body (JSON):**

```json
{
  "prompt": "a golden retriever running on the beach, slow motion, cinematic",
  "model": "veo_31_fast",
  "mode": "text_to_video",
  "aspect_ratio": "16:9",
  "resolution": ["720p", "1080p"]
}
```

**Available video models:**

| Model | Credits | Notes |
|-------|---------|-------|
| `veo_31_fast_relaxed` | 0 | Default, low priority (ULTRA accounts only) |
| `veo_31_fast` | 10 | Normal priority |
| `veo_31_quality` | 100 | Highest quality |

### Step 5.3 — Increase Wait time

Videos take **1–5 minutes** to generate. Set **Wait = 60 seconds** (or more) between status checks.

### Step 5.4 — Remaining nodes

Same as the image workflow: **Check Status** → **IF** → **Download File**.

---

<a name="workflow-batch"></a>

## 6. 📦 Workflow 3: Batch Generation

> **Goal:** Read a list of prompts from Google Sheets / CSV → generate images in bulk.

### 📋 Flow overview

```
[Schedule Trigger] → [Google Sheets: Read prompts] → [Loop Over Items]
                                                           ↓
                              [HTTP: Generate] → [Wait] → [HTTP: Check Status]
                                                           ↓
                                                [IF] → [Save Results]
```

### Step 6.1 — Add trigger node

Choose one:
- **Schedule Trigger**: Auto-run on a schedule (e.g., daily at 6 AM)
- **Webhook Trigger**: Trigger externally (from Telegram, Slack...)
- **Manual Trigger**: Manual execution

### Step 6.2 — Read prompt list

**From Google Sheets:**
1. Add a **Google Sheets** node → select **Read Rows**
2. Connect your Google account
3. Select the spreadsheet containing your prompts
4. Example columns: `A` = Prompt, `B` = Model, `C` = Aspect Ratio

**From CSV/JSON file (alternative):**
1. Add a **Read/Write Files from Disk** node
2. Point to your prompt file

### Step 6.3 — Process each prompt

1. Add a **Loop Over Items** (or **Split In Batches**) node
2. Batch size: `1` (send one prompt at a time to avoid overload)

### Step 6.4 — Submit generation request (inside loop)

Same as Step 4.3, but replace the static prompt with dynamic expressions:

**Body (JSON):**
```json
{
  "prompt": "={{ $json.prompt }}",
  "model": "={{ $json.model || 'imagen4' }}",
  "count": 1,
  "aspect_ratio": "={{ $json.aspect_ratio || '16:9' }}"
}
```

> 💡 `$json.prompt` pulls the prompt value from Google Sheets. `||` sets a default value if the column is empty.

### Step 6.5 — Polling loop + result handling

Same as Steps 4.4–4.8. After retrieving results, connect back to **Loop Over Items** to continue with the next prompt.

---

<a name="error-handling"></a>

## 7. ⚠️ Error Handling & Advanced Tips

### 7.1 Handling failed tasks

Add a branch to handle `failed` status:

1. In the **IF** node "Is Completed?", add a second error check branch
2. Add a second **IF** node after the `false` branch:

| Property | Value |
|----------|-------|
| **Value 1** | `{{ $json.status }}` |
| **Operation** | `is equal to` |
| **Value 2** | `failed` |

3. **true** branch (failed) → Send error notification (Email, Telegram, Slack...)
4. **false** branch (pending/running) → Loop back to **Wait** to continue polling

### 7.2 HTTP Error Codes

| Code | Meaning | Action |
|------|---------|--------|
| `200` | Success | — |
| `202` | Task accepted | Begin polling |
| `400` | Bad request (missing prompt, invalid JSON) | Check request body |
| `401` | Invalid or missing API Key | Check `X-API-Key` header |
| `404` | Task/file not found | Check `task_id` |
| `500` | Server error | Check if G-Labs is still running |

### 7.3 Optimization Tips

| Tip | Details |
|-----|---------|
| **Health check first** | Add a GET `/api/health` node at the start to ensure the server is online |
| **Timeout protection** | Add a counter for poll attempts. If > 20 polls (10 min) → stop and report error |
| **Save credentials** | In n8n, go to **Settings → Credentials** → create **Header Auth** to reuse the API Key |
| **Parallel requests** | Send multiple requests simultaneously (G-Labs manages its own internal queue) |
| **Retry** | Use n8n's **Error Trigger** node + **Retry on Fail** on HTTP Request nodes |

### 7.4 Reusable Credential Setup

Instead of entering the API Key manually on every HTTP Request node:

1. Open an HTTP Request node → open **Authentication**
2. Select **Header Auth**
3. Configure:

| Property | Value |
|----------|-------|
| **Name** | `X-API-Key` |
| **Value** | `PASTE_YOUR_API_KEY` |

4. Save credential → reuse across all HTTP Request nodes

---

<a name="api-reference"></a>

## 8. 📖 API Quick Reference

### Endpoints

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/api/health` | GET | ❌ | Check server status |
| `/api/image/generate` | POST | ✅ | Submit image generation task |
| `/api/video/generate` | POST | ✅ | Submit video generation task |
| `/api/status/{task_id}` | GET | ✅ | Check task status |
| `/api/result/{task_id}` | GET | ✅ | Get task result |
| `/api/files/{filename}` | GET | ❌ | Download file (no auth required) |
| `/api/tasks` | GET | ✅ | List all tasks |

### Task Lifecycle

```
pending → running → completed ✅
                  → failed ❌
```

### Image Models

| `model` value | Description | Supported Ratios |
|---------------|-------------|-----------------|
| `imagen4` | Default, high quality | `1:1`, `3:4`, `4:3`, `9:16`, `16:9` |
| `nano_banana` | Fast, Whisk support | `1:1`, `9:16`, `16:9` |
| `nano_banana_2` | Fast, Flow support | `9:16`, `16:9` |
| `nano_banana_pro` | High quality, Flow support | `9:16`, `16:9` |

### Video Models

| `model` value | Credits | Description |
|---------------|---------|-------------|
| `veo_31_fast_relaxed` | 0 | Default, low priority (ULTRA only) |
| `veo_31_fast` | 10 | Normal priority |
| `veo_31_quality` | 100 | Highest quality |

### Aspect Ratios

| Type | Supported Ratios |
|------|-----------------|
| **Image (imagen4)** | `1:1`, `3:4`, `4:3`, `9:16`, `16:9` |
| **Image (nano_banana)** | `1:1`, `9:16`, `16:9` |
| **Image (nano_banana_2 / pro)** | `9:16`, `16:9` |
| **Video (all models)** | `16:9`, `9:16` |

---

## 🔗 Related Documentation

- 📖 [Webhook API Guide (full)](WEBHOOK_API_GUIDE_EN.md)
- 📖 [G-Labs User Guide](USER_GUIDE_EN.md)
- 🌐 [n8n Documentation](https://docs.n8n.io/)
- 💬 [Discord Community](https://discord.gg/munMZEBMw5)

---

<p align="center">
  <b>G-Labs Automation × n8n</b> — Automated AI Image & Video Generation<br><br>
  🌐 <a href="https://duckmartians.info/">duckmartians.info</a> · 💬 <a href="https://discord.gg/munMZEBMw5">Discord Community</a><br>
  <b>Author: Đặng Minh Đức</b> · <a href="https://github.com/duckmartians">@duckmartians</a>
</p>
