[![Download for Windows](https://img.shields.io/badge/Download%20for%20Windows-%F0%9F%92%BB-blue?style=for-the-badge)](https://github.com/duckmartians/G-Labs-Automation/releases/latest)

Join the G-Labs Automation community here: [https://discord.gg/munMZEBMw5](https://discord.gg/munMZEBMw5)

Hướng dẫn sử dụng: [![Tiếng Việt](https://img.shields.io/badge/Tiếng%20Việt-green)](README_vi.md)

User manual: [![English](https://img.shields.io/badge/English-blue)](README.md) 

# G-Labs Automation - Complete User Guide

**AI Image & Video Generation Automation Tool using Google Labs (Imagen, Veo)**

---

## 🎯 Introduction

G-Labs Automation is a desktop GUI tool that automates AI image and video generation through Google Labs APIs:
- **Imagen 4 / Nano Banana**: Generate images from text or reference images
- **Veo 3.1**: Generate videos from text, images, or components
- **Workflow System**: Create automated pipelines with a node-based editor

### System Requirements
- Windows 10/11
- Google account with access to Google Labs

### ⚠️ Important Security Notice
- This application is developed in Python and Since this is independent software without a Digital Signature, Windows Defender or SmartScreen may mistakenly flag it as a potential threat. This is a common "False Positive".
- Safety Guarantee: This tool is completely clean and safe. If you scan it with specialized antivirus software such as Kaspersky, Bitdefender, or ESET, it will be recognized as SAFE. Please select "Run anyway" or add the file to your exception list to proceed.

---

Run **G-LabsAutomation**

<img width="147" height="162" alt="image" src="https://github.com/user-attachments/assets/754240c1-9924-44ef-9214-7aab59d5cfeb" />

## ⚙️ Initial Setup

### 1. Add Google Account

#### Step 1: Add to Application
1. Go to **⚙️ Settings** tab
2. Click **📋 Add Account**
3. Click **💾 Save**

#### Step 2: Verify
- Account appears in list with status **✅ Ready**
- If error, see [Error Handling](#-error-handling) section

## ACCOUNT UNDERSTANDING

To use the tool effectively and safely, you need to clearly distinguish between **2 types of accounts**:

1. **License Account:**

* This is your primary Google account used to log in to the software for the first time.
* The system will register your subscription plan (Basic/Plus/Max) based on this email.
* **Recommendation:** Use your primary, highly reliable email address to ensure your purchasing rights and long-term support.

2. **Worker Accounts:**
* With Nano Banana and Imagen 4: a regular (free) Gmail account is sufficient for image creation.
* With Nano Banana Pro and Veo 3.1: a Gmail account with a Google One Pro or Ultra plan is required for image creation.
* These are Google (Gmail) accounts added in the *Settings* section to create images/videos.
* The tool supports an **unlimited** number of worker accounts.
* In the future, we will support other platforms, so worker accounts will not be limited to Google.
* **Tip:** You can use secondary or inexpensive accounts to run this feature without affecting your main email account.

---

## SYSTEM SETUP & ADDING ACCOUNTS

Before you begin, load the "ingredients" (worker accounts) for this machine.
1. On the main interface, click the **⚙️ Settings** button or the gear icon in the bottom left corner.
2. Switch to the **"Google Accounts"** tab.
3. **Add Account:**
* Click the **"➕ Add Account"** button. A browser window will appear; simply log in to Gmail as usual. The tool will automatically capture the Cookie and Token.
4. **Proxy Configuration (For Professionals):**
* To manage a large number of accounts and run multiple threads without being blocked by Google IP, you should assign a Proxy to each account.
* Click the "Edit" icon (pencil) next to the account to add a Proxy (HTTP/SOCKS5).
> **Optimal Feature:** The tool has an **Auto-Renew Token** mechanism. When the Google Token expires, the tool will automatically open a background browser to renew the session, ensuring uninterrupted 24/7 operation.

---

# Guide: Fix Issue Creating Images/Videos (Nano Banana Pro & Veo 3.1)

## Problem

If you are unable to generate **images with nano banana pro** or **videos with veo 3.1**,  
there is a high chance that **your device has been restricted by Google**.

This restriction may persist even if you try:
- Fake IP
- VPN
- Proxy
- DNS changes

In most cases, these methods do **not** work reliably.

## Working Solution

The only method that currently works (tested) is using **Proton VPN (Free version)**.

🔗 https://protonvpn.com/free-vpn

### Why Proton VPN?
- Completely **free** to use
- No credit card required
- Stable and reliable
- Paid plans are also **very affordable** if you need more features

## Steps

1. Download and install **Proton VPN**
2. Use the **Free plan**
3. Connect to any available free server
4. Retry creating images with **nano banana pro** or videos with **veo 3.1**

## Notes

- This workaround may not be permanent
- Effectiveness depends on Google’s current restriction policies
- If one server does not work, try reconnecting to another free server

## Conclusion

If your device is blocked and nothing else works,  
**Proton VPN Free** is currently the most effective solution to bypass the limitation.

Give it a try.


---

### 2. Configure Proxy (Optional)

To use proxy for avoiding rate limits or geo-restrictions:

**Proxy Format:**
```
# HTTP/HTTPS
http://username:password@proxy.example.com:8080

# SOCKS5
socks5://username:password@proxy.example.com:1080

# No auth
http://proxy.example.com:8080
```

**How to Add:**
1. Go to **⚙️ Settings** tab
2. Select account to add proxy
3. Click **✏️ Edit**
4. Enter proxy in **Proxy** field
5. Click **💾 Save**

---

### 3. General Settings

Go to **⚙️ Settings** → **General Settings** section:

| Option | Description | Recommended Value |
|--------|-------------|-------------------|
| **Language** | Interface language (Vietnamese/English) | `English` |
| **Output Folder** | Folder to save images/videos | [./output](file:///d:/Data_Online/G-Labs%20Automation/DEV/PC-Home/G-Labs%20Automation%20-%20backup%208%20-%20Copy/src/ui/workflow/node_editor.py#541-544) |
| **Max Threads** | Number of concurrent processing threads | `3` (Plus/Max) |
| **Auto Upscale** | Auto upscale images to 1080p | `✅` |
| **Retry Count** | Number of retries on error | `3` |

**Note:**
- Free accounts: 1 thread
- Plus accounts: 6 threads
- Max accounts: no limit on the number of concurrent threads

---

## 🖼️ Image Generation with Imagen

### Nano Banana (Standard)

Generate images from text prompt + up to 4 reference images (2 Subject, 1 Scene, 1 Style).

#### Step 1: Select Model
Go to **🎨 Image Generation** tab → Select **Nano Banana**

#### Step 2: Enter Prompt
```
Prompt: A futuristic cyberpunk city at night with neon lights
```

**Prompt Writing Tips:**
- Be clear and detailed
- Add style keywords: `realistic, anime, oil painting, 3D render`
- Length: 10-100 words

#### Step 3: Add Reference Images (Optional)

**Subject (Main subject - Max 2):**
```
📷 Subject 1: [person.jpg]
📷 Subject 2: [object.jpg]
```

**Scene (Background - Max 1):**
```
🌆 Scene: [background.jpg]
```

**Style (Art style - Max 1):**
```
🎨 Style: [art_style.jpg]
```

**How to add:**
1. Click **📁 Select Image** next to each Reference
2. Choose image file (JPG, PNG, WebP)
3. Image shows preview

#### Step 4: Configuration

| Option | Values |
|--------|--------|
| **Aspect Ratio** | `1:1 Square` / `16:9 Landscape` / `9:16 Portrait` |
| **Resolution** | `720p` / `1080p (Upscale)` |
| **Number of Images** | `1-4` |
| **Seed Mode** | `Random` / `Fixed` |

#### Step 5: Generate
1. Click **▶️ Generate Image**
2. Monitor progress in **Log**
3. Images saved to **Output Folder**

**Output Files:**
```
output/
├── 01_A_futuristic_cyberpunk_city_subject1_20260108_220000.jpg
├── 02_A_futuristic_cyberpunk_city_subject1_20260108_220001.jpg
└── ...
```

---

### Nano Banana Pro (Advanced)

Generate images with up to **10 reference images** (no categorization required).

#### Key Differences
- ✅ Supports 10 refs (vs 4 in Standard)
- ✅ No need to categorize (Subject/Scene/Style)
- ⚠️ Only supports **Landscape (16:9)**

#### Usage
1. Go to **🎨 Image Generation** → **Nano Banana Pro**
2. Enter prompt
3. Click **📁 Add Reference** (max 10 times)
4. Aspect ratio automatically locks to **16:9**
5. Click **▶️ Generate Image**

---

### Whisk

Generate images from 3 reference images (Subject + Scene + Style) without text prompt.

#### Step 1: Add References
1. Go to **🎨 Image Generation** → **Whisk**
2. Click **📁 Subject** → Select subject image
3. Click **📁 Scene** → Select background image
4. Click **📁 Style** → Select style image

**Requirements:**
- ✅ All 3 references are **required**
- ✅ Prompt is optional

#### Step 2: Configure & Generate
- Select aspect ratio
- Select resolution
- Click **▶️ Generate Image**

---

## 🎬 Video Generation with Veo

Go to **🎥 Video Generation** tab

### 1. Text-to-Video

Generate video from text description.

#### Usage
```
Prompt: A cat walking on a beach during sunset, waves crashing
```

1. Enter prompt describing video
2. Select **Aspect Ratio**: 16:9 / 9:16 / 1:1
3. Select **Resolution**: 720p / 1080p
4. Click **▶️ Generate Video**

**Tips:**
- Describe motion clearly: `walking, running, flying, rotating`
- Add environment details: `during sunset, in the rain, underwater`
- Length: 10-50 words

---

### 2. Image-to-Video

Generate animated video from 1 or 2 images.

#### Mode 1: Single Image
```
Start Image: cat.jpg
Prompt: The cat starts walking forward
```

Video will animate from static image according to prompt.

#### Mode 2: Two Images (Interpolation)
```
Start Image: cat_standing.jpg
End Image: cat_sitting.jpg
Prompt: The cat slowly sits down
```

Video will transition from start → end.

#### Usage
1. Click **📁 Start Image** → Select first image
2. (Optional) Click **📁 End Image** → Select end image
3. Enter prompt describing motion
4. Select aspect ratio & resolution
5. Click **▶️ Generate Video**

---

### 3. Components (3 References)

Generate video from 3 reference images (like Whisk but for video).

#### Requirements
- ✅ 3 references: Subject + Style + Scene
- ⚠️ **Only supports Landscape (16:9)**

#### Usage
1. Click **📁 Subject** → Select subject image
2. Click **📁 Style** → Select style image
3. Click **📁 Scene** → Select scene image
4. Enter prompt describing motion
5. Aspect ratio automatically locks to **16:9**
6. Click **▶️ Generate Video**

## AI VIDEO CREATOR – POWERED BY VEO

A "killer feature" with exceptional resource optimization capabilities.

### 1. Smart Account Filtering

* **Veo 3.1 Fast (Lower Priority) Model:** This tool allows **unlimited video generation** if you own an **ULTRA** Gmail account. This is a massive bargain compared to buying credits on other platforms.
* The tool automatically filters and selects only eligible accounts (Pro/Ultra) for video tasks, ensuring basic accounts aren't wasted on incompatible tasks.

### 2. "Component-based Video" Tab – Intelligent Recognition

This is the smartest feature for Bulk Creation:

* **The Problem:** You have 100 prompts, along with 100 specific Character images and 100 Background images. You want to create 100 matching videos.
* **The Solution:**
* Simply select the folder containing your component images.
* The tool **automatically scans filenames** and compares them with **keywords in your Prompt**.
* *Example:* Prompt is "A cat running in the forest". If the folder contains `cat.png` and `forest.jpg`, the tool **automatically grabs** these 2 images and inserts them into the Reference Image slots for that specific prompt row.
* This eliminates the need to manually select images for every single video.



### 3. Advanced Pair Modes

* **Start - End:** Generates a video transitioning from Image A to Image B.
* **Sequential Chain:** Automatically uses the "End Frame" of Video 1 as the "Start Frame" of Video 2. Extremely useful for creating long, seamless storytelling videos.

---

### Video Output

**File Format:**
```
output/
├── 01_prompt_text_slot1_20260108_220500.mp4
└── ...
```

**Specifications:**
- Duration: ~5 seconds
- FPS: 24
- Codec: H.264
- Resolution: 1280x720 (720p) or 1920x1080 (1080p)

---

## 🔀 Workflow System

Node-based visual programming for complex automation.

### Node Types

#### 1. 📷 Reference (Reference Image)
**Inputs:** None  
**Outputs:** Image  
**Function:** Load image from file as input for other nodes

**Usage:**
1. Right-click canvas → **Add: Reference Standard/Pro**
2. Click **📁 Browse** → Select image
3. Drag wire from output socket

**2 Modes:**
- **Standard**: For Nano Banana (4 refs)
- **Pro**: For Nano Banana Pro (10 refs)

---

#### 2. 🎨 Generate (Image Generation)
**Inputs:** Image (0-10 refs)  
**Outputs:** Image  
**Function:** Generate image from prompt + references

**Widgets:**
- Model: Nano Banana / Nano Banana Pro
- Prompt: Text description
- Aspect Ratio: 1:1 / 16:9 / 9:16
- Resolution: 720p / 1080p
- Seed Mode: Random / Fixed
- Number of Images: 1-4

**Validation Rules:**
- Nano Banana: Max 2 Subject + 1 Scene + 1 Style
- Nano Banana Pro: Max 10 refs, Landscape only

---

#### 3. 💾 Save (Save Image)
**Inputs:** Image  
**Outputs:** None  
**Function:** Save image to file

**Widgets:**
- **📁 Select Folder**: Choose custom save folder
- **Filename Prefix**: File name prefix

**Default:** Saves to shared Output folder

---

#### 4. 📂 Batch Loader (Batch Image Loader)
**Inputs:** None  
**Outputs:** Image (batch)  
**Function:** Load multiple images from folder for sequential processing

**Widgets:**
- **📁 Select Folder**: Choose folder containing images
- **Sort Order**: A-Z / Z-A / Newest / Oldest / Random
- **Limit**: All / Custom number

**Batch Processing:**
```
Folder: /my_images/
  ├── cat1.jpg
  ├── cat2.jpg
  └── cat3.jpg

→ Workflow runs 3 times (1 for each image)
```

---

#### 5. 📝 Batch Prompt (Batch Prompt Loader)
**Inputs:** None  
**Outputs:** Prompt (batch)  
**Function:** Load multiple prompts for sequential processing

**Widgets:**
- **Prompt List**: Enter prompts (one per line)
- **Mode**: Sequential / Random
- **Limit**: All / Custom

**Example:**
```
A cat on the beach
A dog in the park
A bird in the sky
```
→ Workflow runs 3 times

---

#### 6. 🔄 Reroute
**Inputs:** Any  
**Outputs:** Any  
**Function:** Intermediate point for cleaner wire routing

---

### Creating Workflows

#### Example 1: Batch Image Generation
```
[Batch Prompt] → [Generate] → [Save]
```

**Goal:** Generate multiple images from list of prompts

**Steps:**
1. Right-click → **Add: Batch Prompt**
2. Enter prompts (one per line)
3. Right-click → **Add: Generate**
4. Drag wire: Batch Prompt output → Generate prompt input
5. Configure Generate (model, ratio, etc.)
6. Right-click → **Add: Save**
7. Drag wire: Generate output → Save input
8. Click **▶️ Run Workflow**

---

#### Example 2: Reference Image + Batch Prompts
```
[Reference] ──┐
              ├─→ [Generate] → [Save]
[Batch Prompt]┘
```

**Goal:** Create variations of 1 ref image with multiple prompts

**Steps:**
1. Add Reference node → Load image
2. Add Batch Prompt node → Enter 10 prompts
3. Add Generate node
4. Connect:
   - Reference output → Generate image input
   - Batch Prompt output → Generate prompt input
5. Add Save node → Connect
6. Run → Generates 10 image variations

---

### Batch Processing

#### Multiple Batch Nodes

Workflow supports **multiple Batch nodes** simultaneously with **intelligent looping**.

**Example:**
```
[Batch Loader] ──┐ (5 images)
                 ├─→ [Generate] → [Save]
[Batch Prompt] ──┘ (10 prompts)
```

**Result:** 10 iterations (max count)
- Iteration 1: image1 + prompt1
- Iteration 2: image2 + prompt2
- Iteration 3: image3 + prompt3
- Iteration 4: image4 + prompt4
- Iteration 5: image5 + prompt5
- Iteration 6: image1 + prompt6 (loop back)
- ...

**Rule:** Modulo wrapping for shorter lists.

---

### Quick Add Menu

Drag wire from socket outward → Menu appears for quick node addition.

**Usage:**
1. **From OUTPUT socket**: Drag out → Menu → Select node → Auto-connect
2. **From INPUT socket**: Drag out → Menu → Select node → Auto-connect in reverse

**Keyboard Shortcuts:**
- `L`: Auto-arrange nodes
- `Ctrl+C`: Copy selected nodes
- `Ctrl+V`: Paste nodes
- `Ctrl+S`: Save workflow
- `Ctrl+O`: Load workflow
- [Delete](file:///d:/Data_Online/G-Labs%20Automation/DEV/PC-Home/G-Labs%20Automation%20-%20backup%208%20-%20Copy/src/ui/workflow/node_editor.py#143-196): Delete selected

---

### Save & Load Workflow

**Save:**
1. Click **💾 Save Workflow**
2. Choose location & filename (.json)

**Load:**
1. Click **📂 Load Workflow**
2. Select .json file

**Format:**
```json
{
  "nodes": [...],
  "edges": [...],
  "groups": [...]
}
```

---

## 🚀 Advanced Features

### 1. Bulk Prompt Import

Go to **🎨 Image Generation** or **🎥 Video Generation** tab → Click **📥 Import Prompts**

**Supported Formats:**

**TXT File:**
```
A cat on the beach
A dog in the park
A bird in the sky
```

**Excel File (.xlsx):**
| Column A | Column B (ignored) |
|----------|-------------------|
| Prompt 1 | ... |
| Prompt 2 | ... |

**Mode:**
- **Append**: Add to end of current prompts
- Supports Unicode & newlines in Excel cells

---

### 2. Parallel Processing

**Plus/Max Accounts:**
- Configure `Max Threads` = 3-5
- Workflow runs multiple jobs concurrently
- Saves time for large batches

**Smart Delays:**
- Different accounts: 10-20s stagger
- Same account: 20-30s delay

---

### 3. Captcha Queue System

Automatically handles reCAPTCHA in background.

**How it works:**
- Worker maintains 1 Chrome instance
- Queues tokens for workers
- Avoids blocks from too many captcha requests

---

### 4. Unified Filename Convention

All output files:
```
{row:02d}_{prompt}_{slot}_{timestamp}

Example:
01_A_cat_on_beach_subject1_20260108_220500.jpg
```

---

## 🛠️ Error Handling

### Common Errors

#### 1. `401 Unauthorized`
**Cause:** Expired cookies  
**Solution:**
1. Get fresh cookies from browser
2. Update in **Settings** → **Edit account**

---

#### 2. `403 Forbidden / reCAPTCHA Failed`
**Cause:** Google blocking due to spam requests  
**Solutions:**
- Wait 5-10 minutes
- Reduce thread count
- Use proxy
- Refresh browser cookies

---

#### 3. `400 Bad Request - INVALID_ARGUMENT`
**Cause:** Invalid parameters  
**Check:**
- Aspect ratio with model (Pro = Landscape only)
- Reference count (Nano Banana = 4 max, Pro = 10 max)
- Image format (JPG, PNG, WebP)

---

#### 4. Workflow `Missing Input`
**Cause:** Node missing required input  
**Solution:**
- Check all required inputs are connected
- Generate node needs prompt (from widget or socket)

---

#### 5. Cannot Generate Video
**Causes:**
- Components mode + not landscape
- Start image missing (image-to-video mode)

**Solutions:**
- Change aspect ratio to 16:9
- Add start image

---

## ❓ FAQ

### Q: What are Free account limitations?
**A:** 
- 1 thread (no parallel processing)
- Daily quota limited by Google
- All other features available

---

### Q: How to increase speed?
**A:**
1. Upgrade to Plus/Max account
2. Increase `Max Threads` to 3-5
3. Use multiple Google accounts
4. Use proxy to avoid rate limits

---

### Q: Where are images/videos saved?
**A:** 
- Default: [./output/](file:///d:/Data_Online/G-Labs%20Automation/DEV/PC-Home/G-Labs%20Automation%20-%20backup%208%20-%20Copy/src/ui/workflow/node_editor.py#541-544)
- Custom: Configure in **Settings** → **Output Folder**
- Workflow Save node: Can choose separate folder

---

### Q: What size should reference images be?
**A:**
- Min: 200x200px
- Max: 4096x4096px (auto-resize)
- Format: JPG, PNG, WebP
- Recommended: 1024x1024px

---

### Q: Is there a limit on workflow nodes?
**A:** No. However:
- Many complex nodes = slower
- Recommend: < 20 nodes/workflow

---

### Q: Can I export to other video formats?
**A:** 
- Default output: MP4 (H.264)
- To convert: Use external FFmpeg
```bash
ffmpeg -i video.mp4 -c:v libx265 output.mp4
```

---

### Q: How to backup workflows?
**A:**
1. Save workflows as .json files
2. Copy to cloud storage (Google Drive, Dropbox)
3. Version control with Git

---

## 📞 Support

- **Website**: [https://duckmartians.info/](https://duckmartians.info/)
- **Discord**: [https://discord.gg/munMZEBMw5](https://discord.gg/munMZEBMw5)

---

**Created by Đặng Minh Đức [@duckmartians](https://github.com/duckmartians)**
