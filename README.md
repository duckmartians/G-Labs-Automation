# Version v1.0.4 is coming soon. All issues related to image and video creation will be stable. Expected release date: January 8th, 2026.
# Phiên bản v1.0.4 sắp ra mắt. Tất cả các vấn đề liên quan đến việc tạo ảnh và video sẽ được khắc phục ổn định. Ngày phát hành dự kiến: 8 tháng 1 năm 2026.

[![Download for Windows](https://img.shields.io/badge/Download%20for%20Windows-%F0%9F%92%BB-blue?style=for-the-badge)](https://github.com/duckmartians/G-Labs-Automation/releases/latest) [![Download Here](https://img.shields.io/badge/⬇_Download-Here-success?style=for-the-badge)](https://github.com/duckmartians/G-Labs-Automation/releases/download/v1.0.3/Setup_G-Labs_Automation_v1.0.3.exe)

[![Tiếng Việt](https://img.shields.io/badge/Tiếng%20Việt-green)](README_vi.md)     [![English](https://img.shields.io/badge/English-blue)](README.md) 
# COMPREHENSIVE USER GUIDE: G-LABS AUTOMATION v1.0.3

**The Ultimate All-in-One AI Image & Video Automation Tool**

Welcome to **G-Labs Automation** – an advanced solution designed to help you harness the full power of state-of-the-art AI models (Imagen, Veo, Nano Banana) from Google Labs. This tool transforms manual, tedious tasks into intelligent automated workflows, saving you hundreds of hours of work.

Below is the detailed step-by-step guide.

---

## PART 1: DOWNLOAD AND INSTALLATION

### 1. Download the Software

First, download the official installer from our repository:

* **Download Link:** [Portable_G-Labs_Automation_v1.0.3.zip](https://github.com/duckmartians/G-Labs-Automation/releases/download/v1.0.3/Portable_G-Labs_Automation_v1.0.3.zip)

### 2. Installation

1. Extract the downloaded Portable_G-Labs_Automation_v1.0.3.zip file to begin.

2. After installation is complete, select the "Launch G-Labs Automation" checkbox and click **Done**.

> Security & Malware-Free Commitment
> 
> We hereby commit that this software does not contain any virus, malware, spyware, ransomware, or any other malicious components that could harm users’ systems or data.
> 
> This software is developed solely to provide the functionalities explicitly described in its documentation. It does not collect personal data, does not run hidden background processes, does not self-replicate, and does not modify system settings beyond what is strictly required for its intended operation.
> 
> In some cases, antivirus software may flag this application due to the fact that it is custom-developed, independently packaged, not digitally signed with a commercial certificate, and has limited distribution. Modern antivirus solutions rely heavily on heuristic and reputation-based analysis, which can occasionally result in false positives for legitimate software. Such warnings do not indicate malicious behavior.
> 
> We fully support transparency and are willing to cooperate with users by providing independent security scans, technical verification, or additional information to ensure confidence in the safety and integrity of this software.

---

## PART 2: UNDERSTANDING ACCOUNTS (CRITICAL)

To use the tool effectively and safely, you need to distinguish between the **2 types of accounts**:

1. **License Account:**
* This is your personal Google account used to **log in to the software** upon opening.
* The system verifies your subscription plan (Basic/Plus/Max) based on this email.
* **Recommendation:** Use your main, reliable email to ensure purchase rights and long-term support.


2. **Worker Accounts:**
* These are the Google accounts (Gmail) added within the *Settings* menu to perform the actual tasks (generating images/videos).
* The tool supports adding **unlimited** worker accounts.
* **Tip:** You can use secondary accounts or bulk-created accounts for this purpose to protect your main License Account from any potential platform risks.



---

## PART 3: SYSTEM SETUP & ADDING ACCOUNTS

Before starting, you need to fuel the engine by adding Worker Accounts.

1. On the main interface, click the **"⚙️ Settings"** button.
2. Navigate to the **"Google Accounts"** tab.
3. **Add Account:**
* **Method 1 (Automatic):** Click **"➕ Add Account"**. A browser window will appear; simply log in to Gmail as usual. The tool will automatically capture the Cookie and Token.
* **Method 2 (Manual):** Paste the Cookie (JSON or Netscape format) into the text box and click "Add".


4. **Proxy Configuration (For Power Users):**
* To manage a large number of accounts and run multiple threads without IP blocking, assign a Proxy to each account.
* Click the "Edit" icon (pencil) next to an account to add a Proxy (HTTP/SOCKS5).



> **Smart Feature:** The tool features an **Auto-Renew Token** mechanism. When a Google Token expires, the tool will automatically open a background process to renew the Session, ensuring your 24/7 automation workflow is never interrupted.

---

## PART 4: AI IMAGE GENERATOR – COST OPTIMIZATION

This function allows you to generate thousands of images daily with minimal or zero cost.

### 1. Smart Model Selection

* **Imagen 4 & Nano Banana:** This is a key strength! You can use **Free Tier Gmail accounts** to run these models. The tool automatically rotates accounts to maximize the free daily quota.
* **Nano Banana Pro:** High-quality generation requiring a **PRO** or **ULTRA** Gmail account.

### 2. Upscale

* Supports upscaling to **2K** (requires PRO/ULTRA account) and **4K** (requires ULTRA account).
* Delivers significantly sharper results compared to the original generation.

### 3. Queue Manager

* No need to wait for each image. Enter a list of Prompts, configure the count and aspect ratio, then click **"➕ Add to Queue"**.
* Click **"🚀 START NOW"** and let the tool do the work. It automatically processes tasks, retries upon network errors, and skips tasks if a Prompt violates safety policies.

---

## PART 5: WORKFLOW EDITOR – ULTIMATE CUSTOMIZATION (HIGHLIGHT)

Designed for professionals who need absolute control over the creative process. Unlike rigid tools, G-Labs Workflow allows you to "draw" your process:

* **Node-based Interface:** Drag and drop functional nodes and wire them together.
* *Example:* `Batch Loader (Load images from folder)` -> `Reference Node (Use as input)` -> `Generate Node` -> `Save Node`.


* **Extreme Customization:**
* Create a flow that runs 2-3 different models simultaneously to compare results.
* Chain the output of Generation 1 as the input (Reference) for Generation 2 (Advanced Image-to-Image).


* **Batch Processing:**
* **Batch Image Loader:** Automatically scans a local folder, picks images one by one for processing, and loops until the folder is empty.
* **Batch Prompt Loader:** Automatically reads line-by-line from a text file to use as prompts.



> **Why is it smart?** You can save successful Workflows (`.json` files) to reuse later or share with your team. The core logic handles complex threading automatically.

---

## PART 6: AI VIDEO CREATOR – POWERED BY VEO

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

## PART 7: OTHER OPTIMIZATIONS

1. **Multi-threading:**
* Run multiple accounts simultaneously. The MAX plan supports unlimited threads (dependent only on your computer's hardware and the number of accounts you have).


2. **Anti-Detect Technology:**
* Integrated `Playwright` with browser fingerprint masking (Stealth JS) minimizes the risk of Google checkpoints or account locks.


3. **Auto Update System:**
* Never miss a new feature. Upon opening, the tool checks for updates. Future features like **Script Writer** and **Auto Upload** will be delivered seamlessly via this system.



---

**G-Labs Automation** is not just a tool; it is a diligent virtual assistant that multiplies your productivity.

Start today! If you need support or wish to upgrade your License (Plus/Max), please contact us via the information provided in the Settings tab.

*Wish you create amazing masterpieces!*
