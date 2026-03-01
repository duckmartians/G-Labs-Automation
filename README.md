<p align="center">
  <a href="https://github.com/duckmartians/G-Labs-Automation/releases/latest">
    <img src="https://img.shields.io/badge/Download%20for%20Windows-%F0%9F%92%BB-blue?style=for-the-badge" alt="Download for Windows">
  </a>
</p>

<p align="center">
  <a href="README.md"><img src="https://img.shields.io/badge/English-blue" alt="English"></a>
  <a href="README_vi.md"><img src="https://img.shields.io/badge/Tiếng%20Việt-green" alt="Tiếng Việt"></a>
  <a href="https://discord.gg/munMZEBMw5"><img src="https://img.shields.io/badge/Discord-Community-5865F2?logo=discord&logoColor=white" alt="Discord"></a>
</p>

---

# 📖 G-Labs Automation — User Guide

> Complete user guide for **G-Labs Automation** — an automated AI image and video generation application.

> 🎬 **Video Tutorial:** [Watch on YouTube](https://www.youtube.com/watch?v=Oqr7fGJVilE)

## Table of Contents

| # | Section | Description |
|---|---------|-------------|
| 1 | [Overview & Installation](#section-1) | Introduction, requirements, installation |
| 2 | [Account Management](#section-2) | Add, remove, renew Google accounts |
| 3 | [License System](#section-3) | Basic/Plus/Max plans, payment |
| 4 | [AI Image Creator](#section-4) | 3 AI models, config, batch processing |
| 5 | [AI Video Creator](#section-5) | 3 tabs: Text, Compose, Scene Builder |
| 6 | [Workflow Editor](#section-6) | Visual node-based editor |
| 7 | [Additional Features](#section-7) | Webhook API, Settings, Recovery |

---

<a name="section-1"></a>
<details>
<summary><h2>📦 Section 1: Overview & Installation</h2></summary>

### 1.1 Introduction

**G-Labs Automation** is a Windows desktop application that automates AI image and video generation. It supports multiple AI models (Nano Banana, Imagen 4, Nano Banana Pro), batch processing, and advanced automation workflows.

**Key features:**
- **AI Image Creator**: 3 models, reference images, upscaling, batch processing
- **AI Video Creator**: Text-to-Video, Scene Builder, Compose
- **Workflow Editor**: Visual node-based editor for automation pipelines
- **Webhook API**: Local REST API server for external integrations
- **Multi-language**: Supports 9+ languages

### 1.2 System Requirements

| Requirement | Details |
|-------------|---------|
| **OS** | Windows 10/11 (64-bit) |
| **RAM** | Minimum 4GB, recommended 8GB+ |
| **Disk** | Minimum 500MB free (excluding output data) |
| **Network** | Required |

### 1.3 Download & Installation

1. Download **G-Labs-Automation-vX.Y.Z.zip** from the official source
2. Extract to your preferred folder (e.g., `D:\G-Labs Automation\`)
3. Open the extracted folder and run **G-LabsAutomation.exe**

> ⚠️ **Do not** place the application folder in `C:\Program Files` or system directories requiring admin rights.

<details>
<summary>⚠️ Antivirus False Positive Warning (click to expand)</summary>

Since the application is packaged as a packed executable, some antivirus software may **flag it as a false positive**.

**To add a Windows Defender exclusion:**

1. Open **Windows Security** → **Virus & threat protection**
2. Click **Manage settings** → scroll to **Exclusions**
3. Click **Add an exclusion** → **Folder** → Select the G-Labs Automation folder

> 💡 Add the exclusion **before extracting** to prevent file deletion.

</details>

### 1.4 First Launch

- **Splash Screen**: Shows loading progress while loading accounts and checking license
- **Language Selection**: First launch prompts for language (changeable later in Settings)

### 1.5 Main Interface

| Area | Description |
|------|-------------|
| **Sidebar (left)** | Navigation: Create Image, Create Video, Workflow, Webhook + Settings/Activity/Community |
| **Content Area (right)** | Selected page content |



| Plan Badge | Color |
|------------|-------|
| 🌱 BASIC | Gray |
| 💎 PLUS | Blue |
| 👑 MAX | Purple |

Supports **Dark / Light mode** (toggle at bottom of sidebar) and **notification bar** for system messages + auto-updates.

</details>

---

<a name="section-2"></a>
<details>
<summary><h2>👤 Section 2: Account Management</h2></summary>

Account management is in **Settings → Accounts tab**.

> ⚠️ At least **one valid Google account** with an active session is required to use image/video generation.

### 2.1 Adding Accounts

**Browser Login (recommended):**
1. Click **"Login via browser"**
2. Sign in to Google in the Chromium window
3. The system automatically extracts session data, email, account tier, and credits

**Manual cookie import:** Paste cookies from your browser into the input field.

### 2.2 Account Table (9 columns)

| Column | Description |
|--------|-------------|
| Toggle | Enable/disable account |
| Email | Email address |
| Tier | Free / Pro / Ultra |
| Credits | Remaining credits |
| Proxy | Per-account proxy |
| Cookie Exp | Session expiry date |
| Token Exp | Auth token expiry |
| Status | ✅ Valid / ❌ Expired / 🔄 Checking / ⚠️ Duplicate |
| Actions | Renew / Edit / Delete |

### 2.3 Session Renewal

- **Step 1**: Cookie refresh (opens browser → re-login)
- **Step 2**: Tier check (API call → new token + tier + credits)
- Supports **batch renewal** (all accounts) and **auto-renew** at startup (Plus/Max)

### 2.4 Edit & Delete

- **Edit**: View/edit per-account proxy (`IP:Port` or `IP:Port:User:Pass`)
- **Delete**: Permanently removes account + browser profile

</details>

---

<a name="section-3"></a>
<details>
<summary><h2>🔑 Section 3: License System</h2></summary>

License is managed in **Settings → License tab**.

### 3.1 Plan Comparison

| Feature | 🌱 Basic | 💎 Plus | 👑 Max |
|---------|---------|---------|--------|
| **Threads** | 1 | 10 | ♾️ |
| **Queue** | 1 task | 10 tasks | ♾️ |
| **Images/Prompt** | 1 | 4 | 4 |
| **Videos/Prompt** | 1 | 4 | 4 |
| **Prompt limit** | 10 lines | ♾️ | ♾️ |
| **Proxy Pool** | ❌ | ✅ | ✅ |
| **Image Upscale** | ❌ | ✅ (2K+4K) | ✅ (2K+4K) |
| **Video Upscale** | ❌ | ✅ | ✅ |
| **Full Video Modes** | Start+End only | ✅ | ✅ |
| **Workflow** | ❌ | ✅ | ✅ |
| **Error Retry** | ❌ | ✅ | ✅ |
| **Auto-Renew** | ❌ | ✅ | ✅ |
| **Webhook API** | ❌ | ❌ | ✅ |

### 3.2 Login

1. Click **"Login with Google"**
2. Select account → authenticate → receive plan, user ID, expiry date
3. Session saved locally, no need to re-login

> ⚠️ Each license works on **one device only**. Logging in on another device will sign out the previous one.

### 3.3 Pricing

<details>
<summary>💳 Pricing & payment methods (click to expand)</summary>

**PLUS 💎 Plan:**

| Duration | USD |
|----------|-----|
| 1 month | $3 |
| 6 months | $15 |
| 1 year | $30 |

**MAX 👑 Plan:**

| Duration | USD |
|----------|-----|
| 1 month | $6 |
| 6 months | $30 |
| 1 year | $60 |

**Payment methods:**
- 🇻🇳 **VietQR bank transfer** (VPBank, auto-generated QR code)
- 🌍 **International cards** (Visa/MC/Apple Pay/Google Pay via Polar)

</details>

### 3.4 Auto-Check

The system automatically checks license status periodically (silently, no popups).

</details>

---

<a name="section-4"></a>
<details>
<summary><h2>🖼️ Section 4: AI Image Creator ⭐</h2></summary>

### 4.1 Interface

| Area | Description |
|------|-------------|
| **Left panel** | Configuration + Prompt input + Control buttons |
| **Right panel** | 7-column prompt table + Toolbar |

### 4.2 Three AI Models

| Feature | 🍌 Nano Banana | 🖼️ Imagen 4 | 🍌 Nano Banana Pro |
|---------|---------------|-------------|-------------------|
| **Reference mode** | Whisk — categorized | Whisk — categorized | Flow — uncategorized |
| **Max references** | 5 (Subject ×3, Scene ×1, Style ×1) | 5 (Subject ×3, Scene ×1, Style ×1) | 10 (generic, uncategorized) |
| **Captcha** | ❌ | ❌ | ✅ Automatic |
| **Aspect Ratio** | 16:9, 9:16, 1:1 | 16:9, 9:16, 4:3, 3:4, 1:1 | 16:9, 9:16 |

### 4.3 Configuration

| Setting | Details |
|---------|---------|
| **Model** | Nano Banana (default), Imagen 4, Nano Banana Pro |
| **Quality** | 1K (original), 2K (Plus/Max), 4K (Plus/Max + Ultra account) |
| **Aspect Ratio** | Depends on model (see table above) |
| **Images/Prompt** | 1-4 (Basic: 1 only) |
| **Threads** | Basic: 1 / Plus: 10 / Max: ♾️ |
| **Delay** | 2-3s default, range 1-300s |
| **Reference Mode** | Default, 1 for all (Plus/Max), Sequential (Plus/Max) |
| **Lock Seed** | 6-digit number for reproducible results |
| **Import** | TXT, Excel (.xlsx/.xls) |
| **Save Mode** | No subfolder / By task / By prompt |

### 4.4 Auto-Match References

Automatically assigns reference images from a folder based on **keywords in the filename** matching the prompt.

- Keywords split by `_`, minimum 3 characters
- Example: `red_car.png` → keywords `red_car`, `red`, `car`

### 4.5 Prompt Table (7 columns)

| Column | Description |
|--------|-------------|
| ☑️ Select | Check/uncheck row |
| # | Row number |
| Task | Queue task name |
| Reference | Reference images (interactive widget) |
| Prompt | Content (editable inline) |
| Output | Generated image (thumbnail) |
| Progress | Status + Retry/Open Folder buttons |

**Toolbar:** Add Row, Add Images, Delete, Clear All, Retry Errors, Run Selected, Load Session, Filter (All/Waiting/Running/Success/Error)

### 4.6 Queue Manager

- **Add to Queue**: Save current configuration as a task
- **Queue Manager**: Rename, Skip, Retry, Delete, Edit tasks
- **Limits**: Basic 1 / Plus 10 / Max ♾️

### 4.7 Session & Backup

- Auto-backup after each change (max 10 files)
- Load Session: restore or export to Excel

</details>

---

<a name="section-5"></a>
<details>
<summary><h2>🎬 Section 5: AI Video Creator 🎬</h2></summary>

### 5.1 Interface

| Area | Description |
|------|-------------|
| **Left panel** | Configuration + Control buttons |
| **Right panel** | 3 tabs + Toolbar |

> ⚠️ Requires a Google account with **PRO** or **ULTRA** tier. Fast [0 Credit] model supports **ULTRA** only.

### 5.2 Three Video Models

| Model | Credits | Description |
|-------|---------|-------------|
| **Fast [0 Credit]** | 0 | Fast, low priority (ULTRA only) |
| **Fast [10 Credit]** | 10 | Fast, normal priority (default) |
| **Quality [100 Credit]** | 100 | Highest quality (no 3-image compose) |

**Generation modes:** Text-to-Video, Start Image, Start+End Image, 3-Image Compose

### 5.3 Configuration

| Setting | Details |
|---------|---------|
| **Resolution** | 720p (original), 1080p (Plus/Max), 4K (Plus/Max) |
| **Aspect Ratio** | 16:9, 9:16 |
| **Videos/Prompt** | 1-4 (Basic: 1 only) |
| **Threads** | Basic: 1 / Plus: 10 / Max: ♾️ |
| **Delay** | 10-20s default |

### 5.4 Pair Mode (Tab 1: Text-to-Video / Image-to-Video)

| Mode | Description |
|------|-------------|
| **Start Image - End Image** | 1:1 pairing |
| **1 Start Image - Multiple End Images** | 1 start image → N end images (Plus/Max) |
| **Multiple Start Images - 1 End Image** | N start images → 1 end image (Plus/Max) |
| **Sequential Chain (1:2 -> 2:3...)** | Chain: end frame N = start frame N+1 (Plus/Max) |

### 5.5 Tab 2: Component-based Video ⭐ (Plus/Max)

Combines **3 component images** into video. Supports auto-match keywords from folder.

### 5.6 Tab 3: Scene Builder 🎞️ (Plus/Max)

**Sequential** video chain — last frame of video N = start image of video N+1.

| Feature | Details |
|---------|---------|
| Resolution | 720p only |
| Threads | 1 only (sequential) |
| Reference images | First video only |

### 5.7 Plan Restrictions

| Feature | Basic | Plus | Max |
|---------|-------|------|-----|
| Tab 1 (Text-to-Video / Image-to-Video) | ✅ | ✅ | ✅ |
| Tab 2 (Component-based Video) | ❌ | ✅ | ✅ |
| Tab 3 (Scene Builder) | ❌ | ✅ | ✅ |
| Resolution | 720p | +1080p+4K | +1080p+4K |

</details>

---

<a name="section-6"></a>
<details>
<summary><h2>🔗 Section 6: Workflow Editor 🔗</h2></summary>

A visual node-based editor — connect processing steps into an automated pipeline.

> ⚠️ Workflow Editor requires **Plus or Max** plan to run.

### 6.1 Interface

| Area | Description |
|------|-------------|
| **Toolbar** | Save Flow, Open Flow, Auto Arrange, Stop, Run Flow |
| **Canvas** | Node placement and connection area |
| **Minimap** | Overview of the full workflow |
| **Log Panel** | Execution logs |

### 6.2 Interactions

| Action | Effect |
|--------|--------|
| Right-click | Add node menu |
| Drag & drop | Move nodes |
| Scroll wheel | Zoom in/out |
| Ctrl+Z/Y | Undo/Redo |
| Ctrl+C/V | Copy/Paste |
| Delete | Remove node/connection |

### 6.3 Node Types

| Node | Input | Output | Description |
|------|-------|--------|-------------|
| 📦 **Batch Image Loader** | — | Image | Load images from folder (sort: A-Z, newest/oldest, random, match prompt) |
| 📝 **Batch Prompt Loader** | — | Prompt | Prompt list (sequential/random, with count limit) |
| 🖼 **Generate Image** | Ref, Prompt | Image | Generate image (3 models, ratio, seed, resolution) |
| 📎 **Reference Image** | Image | Ref | For Nano Banana / Imagen 4 — categorized (Subject/Scene/Style) |
| 📎 **Reference Image (Pro)** | Image | Ref | For Nano Banana Pro — uncategorized |
| 💾 **Save Image** | Image | — | Save image (prefix + folder) |
| 🔀 **Reroute** | Any | Any | Redirect connections |

### 6.4 Execution

Uses **topological sort** to determine execution order. Node states: 🟡 Running / 🟢 Success / 🔴 Error / Purple = Skipped / Gray = Disabled.

### 6.5 Save / Load

- Save and open **JSON** files
- Default workflow auto-loads when app starts

</details>

---

<a name="section-7"></a>
<details>
<summary><h2>🔧 Section 7: Additional Features 🔧</h2></summary>

### 7.1 Webhook API Server (Max only)

Local REST API server for external tools (n8n, Make.com, Zapier, Python...).

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/api/health` | GET | ❌ | Server health check |
| `/api/image/generate` | POST | ✅ | Submit image generation request |
| `/api/status/{task_id}` | GET | ✅ | Check task status |
| `/api/result/{task_id}` | GET | ✅ | Get result |
| `/api/files/{filename}` | GET | ❌ | Download image |
| `/api/tasks` | GET | ✅ | List all tasks |

**Auth:** Header `X-API-Key: YOUR_KEY` | **Port:** 1024-65535 (default 8765)

```json
// POST /api/image/generate
{
  "prompt": "image description",
  "model": "imagen4",
  "aspect_ratio": "16:9"
}
```

### 7.2 Settings (5 Tabs)

| Tab | Content |
|-----|---------|
| **Google Accounts** | Add/remove/renew Google accounts |
| **Proxy Pool** | Custom proxies (HTTP/SOCKS5) + Auto-Rotate |
| **License & Upgrade** | Login/purchase Plus/Max, periodic auto-check |
| **General Settings** | Language (9+), dark/light theme, author info |
| **Logs** | Detailed activity logs |

### 7.3 Recovery & Error Handling

| Error Type | Auto-Recovery |
|------------|---------------|
| 403 Forbidden | Refresh session → rotate account |
| Timeout | Retry up to 3 times |
| Captcha | Browser auto-solves, 10 tokens/session |
| No accounts available | Stops after 5 consecutive failures |

### 7.4 Auto-Update

App checks version from server → shows notification → downloads → replaces files → restarts.

</details>

---

## ✅ Tips to Reduce 403 Errors

### ✅ Fixed
- Reduced **403 errors** by adding more **regular email addresses** 📧
- These emails are **only used to run the captcha code** 🤖

### 🔄 Changed
- In **account settings**, under the **options** section ⚙️:
  - **Disable image** or **video processing** 🖼️🎬
  - After disabling, the system will only execute the **captcha code**, without processing images or videos
![image](https://github.com/user-attachments/assets/a30700b4-07da-4a2d-9f36-c72aaea9120b)

### 📝 Notes
- More email addresses ➜ lower chance of encountering **403 errors** 🚫
- This setup **does not affect** any features outside of captcha

---

<p align="center">
  <b>G-Labs Automation</b> — Batch AI image & video generation<br><br>
  🌐 <a href="https://duckmartins.info/">duckmartians.info</a> · 💬 <a href="https://discord.gg/munMZEBMw5">Discord Community</a><br>
  <b>Author: Đặng Minh Đức</b> · <a href="https://github.com/duckmartians">@duckmartians</a>
</p>
