# TelegramDL 🚀

Serverless Telegram bots using Google Colab and yt-dlp to download and process large videos.

This repository contains two different approaches to bypass local bandwidth constraints and Telegram's upload limits by running the heavy extraction and processing entirely on Google Colab's gigabit backend.

## 📂 Included Notebooks

### 1. `Telegram_DL_SmartSplitBot.ipynb` (Recommended for Standard Accounts)
Uses a standard Telegram Bot API token. Since standard bots are restricted to a 50MB upload limit, this script uses `yt-dlp` to download the full video to Colab, and then leverages a smart FFmpeg subprocess to automatically slice the video into numbered chunks (e.g., "Part 1 of 4") that safely fit under 50MB before uploading them to your chat.

### 2. `Telegram_DL_MTProtoUserbot.ipynb` (Recommended for Large Single Files)
Uses Pyrogram to log into a secondary Telegram account (acting as a Userbot). Because it operates as a standard user, it inherits Telegram's **2GB premium upload limit**, completely eliminating the need for FFmpeg splitting. 

---

## ✨ Core Features
* **Zero Local Bandwidth:** All downloading, FFmpeg processing, and uploading runs on Google Colab.
* **Authentication Bypass:** Reads a `cookies.txt` file directly from Google Drive to bypass age gates, Cloudflare firewalls, and login walls on X (Twitter) and tube sites.
* **Anti-Scraping Evasion:** Uses `curl_cffi` and the developer branch of `yt-dlp` to spoof Chrome browser fingerprints.
* **Secure Credential Storage:** Uses Colab Secrets to keep your API tokens and User IDs completely hidden from the public code.

---

## 🛠️ Step-by-Step Setup Guide

### 1. Prepare Google Drive (For Cookies & Sessions)
1. Open your Google Drive and create a new folder named exactly **`BotData`**.
2. Go to the websites you want to download from on your desktop browser and pass any age gates or bot checks.
3. Use a browser extension (like "Get cookies.txt LOCALLY") to export a `cookies.txt` file. 
4. Upload `cookies.txt` directly into the `BotData` folder on your Google Drive.

### 2. Configure Google Colab Secrets (Crucial for Security)
To ensure your API keys are not exposed when sharing or saving this notebook:
1. Open the `.ipynb` notebook in Google Colab.
2. Click the **Key icon (Secrets)** on the left sidebar.
3. Add the following secrets depending on which bot you are running:
   * `USER_ID` = Your personal Telegram ID (e.g., `1392603898`). Get this from @userinfobot.
   * `BOT_TOKEN` = Your BotFather token (Only needed for the SmartSplit bot).
   * `API_ID` = Your MTProto API ID from my.telegram.org (Only needed for the Userbot).
   * `API_HASH` = Your MTProto API Hash (Only needed for the Userbot).
4. Toggle the switch next to each secret to grant the notebook access.

### 3. Deploy and Run
1. Go to the top menu in Colab and click **Runtime > Run all**.
2. Colab will prompt you to authorize access to your Google Drive. Accept it so the script can read your `cookies.txt`.
3. **For Userbot:** The Colab output console will ask for your secondary account's phone number and login code on the first run. After that, the session is saved to your Drive and bypasses login.
4. **For BotFather:** It will start instantly. 
5. Open Telegram, send a video link to your bot (or your Userbot account), select the quality, and wait for the upload!

---

## ⚠️ Troubleshooting
* **`HTTP Error 403: Forbidden`:** The target site updated its Cloudflare protection, or your cookies expired. Export a fresh `cookies.txt` from your browser and overwrite the old one in Google Drive.
* **`Unable to extract hash`:** The site changed its backend code. Restart the Colab session and run the cell again to fetch the absolute latest developer patch of `yt-dlp` from the master branch.
* **Bot stops responding:** Google Colab terminates idle runtimes after a period of inactivity. Simply reopen the notebook and press Play to wake the bot back up.

## License
Open-source under the MIT License.
