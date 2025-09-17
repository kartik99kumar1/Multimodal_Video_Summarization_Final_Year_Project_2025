# 🎥 Multi‑Modal Video Summarization

Single‑file **frontend** + **Colab/Flask backend** that runs one of three notebooks depending on user choice:

* **Full Video Summarization** (`mode=full`)
* **Time‑Stamp Based Summarization** (`mode=timestamp`)
* **Presenter‑Image Based Summarization** (`mode=presenter`)

This README is copy‑paste ready for your GitHub repo — clear steps, commands, examples and troubleshooting. Use it as `README.md`.

---

## 📑 Table of contents

* [What is this](#-what-is-this)
* [Features 🚀](#-features-)
* [Repository structure 📂](#-repository-structure-)
* [Prerequisites ⚙️](#-prerequisites-️)
* [Backend (Google Colab) — Quick start ✍️](#-backend-google-colab--quick-start-️)
* [Frontend — index.html 🔗](#-frontend--indexhtml-)
* [How modes & file paths map 🗂](#-how-modes--file-paths-map-)
* [Testing the end‑to‑end flow ✅](#-testing-the-endtoend-flow-)
* [Troubleshooting & common fixes 🛠](#-troubleshooting--common-fixes-)
* [Security & production notes 🔒](#-security--production-notes-)
* [Helpful git commands for GitHub 📦](#-helpful-git-commands-for-github-)
* [Next steps & ideas ✨](#-next-steps--ideas-)

---

## 🤔 What is this

This project automatically summarizes long lecture/classroom videos into shorter, focused versions. Users can choose between:

* **Automatic full summarization**
* **Timestamp‑based custom summarization** (via uploaded `time.txt`)
* **Presenter‑image based summarization** (uploading the lecturer’s photo)

---

## 🚀 Features

* 🔗 **Frontend**: single HTML file (`index.html`) — no build system required
* ⚡ **Backend**: Flask + ngrok, runs directly in Google Colab
* 📝 **Multiple summarization modes** supported
* ⏱ **Progress polling** and **estimated time display**
* 📂 Clear input/output paths for reproducibility

---

## 📂 Repository structure

```bash
.
├── index.html                     # Frontend UI
├── backend_server.py              # Flask + Colab backend
├── Full_Video_Summarization_final.ipynb
├── Time_Stamp_Based_Final_.ipynb
├── Presenter_Image_Based_Summarization.ipynb
└── README.md                      # This file
```

---

## ⚙️ Prerequisites

* Google account (for Colab)
* Git + GitHub (for version control)
* Modern browser (Chrome/Edge)

---

## ✍️ Backend (Google Colab) — Quick start

1. Open Colab → upload `backend_server.py` + notebooks.
2. Run first cell to install requirements:

   ```bash
   !pip install -q flask flask-cors pyngrok
   !apt-get -qq update && apt-get -qq install -y ffmpeg
   ```
3. Start server:

   ```bash
   python backend_server.py
   ```
4. Copy the **PUBLIC URL** printed by ngrok → paste into `index.html` as `BACKEND_URL`.

---

## 🔗 Frontend — index.html

* Pure HTML + JS — just open in browser.
* Offers three modes: **Full**, **Timestamp**, **Presenter Image**.
* Upload fields dynamically appear depending on mode.

---

## 🗂 How modes & file paths map

| Mode      | Notebook                                    | Input(s)                     | Raw output path                    |
| --------- | ------------------------------------------- | ---------------------------- | ---------------------------------- |
| full      | `Full_Video_Summarization_final.ipynb`      | `test.mp4`                   | `/content/summary_video.mp4`       |
| timestamp | `Time_Stamp_Based_Final_.ipynb`             | `test.mp4` + `time.txt`      | `/content/timestamp_requested.mp4` |
| presenter | `Presenter_Image_Based_Summarization.ipynb` | `test.mp4` + presenter image | `/content/presenter_requested.mp4` |

All raw outputs are re‑encoded to `/content/output/summary_fixed.mp4` for browser compatibility.

---

## ✅ Testing the end‑to‑end flow

1. Open `index.html`
2. Select **Full Video Summarization** → upload a video → click **Get Video**
3. Wait \~5 minutes → summary video + text appear
4. Repeat with **Timestamp mode** (upload `time.txt`) and **Presenter mode** (upload lecturer’s photo)

---

## 🛠 Troubleshooting & common fixes

* **Video not loading?** → check `/check` endpoint logs
* **Colab timeout?** → increase notebook timeout in backend (`--ExecutePreprocessor.timeout`)
* **ngrok error?** → regenerate auth token & rerun first cell

---

## 🔒 Security & production notes

* ngrok exposes a public URL — for **demo only** ⚠️
* For production → deploy Flask on a secure server (GCP, AWS, Azure)
* Sanitize all file uploads before saving

---

## 📦 Helpful git commands for GitHub

```bash
# Clone repo
git clone https://github.com/yourname/video-summarization.git

# Create new branch
git checkout -b feature-x

# Add + commit + push
git add .
git commit -m "Added timestamp summarization"
git push origin feature-x
```

---

## ✨ Next steps & ideas

* Add **OCR‑based title extraction** (planned)
* Improve UI with progress bar animations
* Add support for multi‑language transcription
* Deploy backend to **cloud server** for persistent use

---

## Author ✍️

**Kartik Kumar**
USN: `1CD22CS061`
Cambridge Institute of Technology, Bangalore



