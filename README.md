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

## 📞 Support

- **Website**: [https://duckmartians.info/](https://duckmartians.info/)
- **Discord**: [https://discord.gg/munMZEBMw5](https://discord.gg/munMZEBMw5)

---

**Created by Đặng Minh Đức [@duckmartians](https://github.com/duckmartians)**
