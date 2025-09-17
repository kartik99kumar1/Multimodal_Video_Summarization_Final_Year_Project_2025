# 🎬 MultiModal Video Summarization

> Single-file frontend + Colab/Flask backend that runs one of three notebooks depending on user choice:
>
> * **Full Video Summarization** (`mode=full`)
> * **Time-Stamp Based Summarization** (`mode=timestamp`)
> * **Presenter-Image Based Summarization** (`mode=presenter`)

This README is copy-paste ready for your GitHub repo — clear steps, commands, examples and troubleshooting. Use it as `README.md`.

---

## 📑 Table of contents

* [What is this ❓](#what-is-this-)
* [Features 🚀](#features-)
* [Repository structure 📁](#repository-structure-)
* [Prerequisites ⚙️](#prerequisites-️)
* [Backend (Google Colab) — Quick start 🧪](#backend-google-colab---quick-start-)
* [Frontend — index.html 🔗](#frontend---indexhtml-)
* [How modes & file paths map 🔄](#how-modes--file-paths-map-)
* [Testing the end-to-end flow ✅](#testing-the-end-to-end-flow-)
* [Troubleshooting & common fixes 🛠️](#troubleshooting--common-fixes-)
* [Security & production notes 🔒](#security--production-notes-)
* [Helpful git commands for GitHub 📦](#helpful-git-commands-for-github-)
* [Next steps & ideas ✨](#next-steps--ideas-)
* [License 📄](#license-)

---

## ❓ What is this

This project provides a simple, attractive frontend (`index.html`) that uploads a `test.mp4` video and triggers server-side Colab notebooks via a small Flask server running in Google Colab (exposed with ngrok). The backend chooses one of three notebooks based on the `mode` parameter and returns a downloadable summary video + text.

---

## 🚀 Features

* Single-file frontend with polished UI and animations.
* Upload `test.mp4` and choose between:

  * **Full summarization**
  * **TimeStamp based summarization** (upload `time.txt`)
  * **Presenter image based summarization** (upload image)
* Backend runs the corresponding Colab notebook and re-encodes outputs for browser download.
* Polling (`/check`) to update frontend when results are ready.
* Outputs served at `/output/summary_fixed.mp4` and `/output/summary.txt`.

---

## 📁 Repository structure

```
.
├── frontend/
│   └── index.html
├── backend/
│   └── colab_server.ipynb   # or a .py Flask script to run in Colab
├── notebooks/
│   ├── Full_Video_Summarization_final.ipynb
│   ├── Time_Stamp_Based_Final_.ipynb
│   └── Presenter_Image_Based_Summarization.ipynb
├── .gitignore
└── README.md
```

---

## ⚙️ Prerequisites

* Google account (for Colab).
* `ngrok` authtoken (optional if using pyngrok from Colab which can request it).
* Modern browser for frontend.
* The frontend `BACKEND_URL` must be set to the ngrok public URL printed by the Colab backend (no trailing slash).

---

## 🧪 Backend (Google Colab) — Quick start

1. Open a new Colab and paste the provided backend server code into a cell (the Flask + pyngrok script).
2. Make sure notebook paths at the top match your actual notebook filenames:

   ```python
   NOTEBOOK_FULL      = '/content/Full_Video_Summarization_final.ipynb'
   NOTEBOOK_TIMESTAMP = '/content/Time_Stamp_Based_Final_.ipynb'
   NOTEBOOK_PRESENTER = '/content/Presenter_Image_Based_Summarization.ipynb'
   ```
3. Run the cell. It installs dependencies, starts Flask and opens an ngrok URL. Colab prints something like:

   ```
   🚀 PUBLIC URL: https://abcd-1234.ngrok-free.app
   ```
4. Copy that URL and set in your frontend `index.html` as `BACKEND_URL`.

**Important Colab file paths (canonical):**

* Input video: `/content/input/test.mp4`
* Timestamp file: `/content/input/time.txt`
* Presenter image: `/content/input/presenter_image.<ext>`
* Raw outputs:

  * Full: `/content/summary_video.mp4`
  * Timestamp: `/content/timestamp_requested.mp4`
  * Presenter: `/content/presenter_requested.mp4`
* Final re-encoded output: `/content/output/summary_fixed.mp4`
* Final text summary: `/content/output/summary.txt`

---

## 🔗 Frontend — index.html

* Replace `BACKEND_URL` near the top of `index.html` with your ngrok URL, e.g.

  ```javascript
  const BACKEND_URL = 'https://abcd-1234.ngrok-free.app';
  ```
* Open `index.html` in your browser, upload files, and choose the mode.

---

## 🔄 How modes & file paths map

* **`mode=full`**

  * Notebook: `NOTEBOOK_FULL`
  * Output: `/content/summary_video.mp4`
* **`mode=timestamp`**

  * Notebook: `NOTEBOOK_TIMESTAMP`
  * Input: `/content/input/time.txt`
  * Output: `/content/timestamp_requested.mp4`
* **`mode=presenter`**

  * Notebook: `NOTEBOOK_PRESENTER`
  * Input: `/content/input/presenter_image.<ext>`
  * Output: `/content/presenter_requested.mp4`

**Backend behavior:** re-encodes the raw file → `/content/output/summary_fixed.mp4` + `/content/output/summary.txt`.

---

## ✅ Testing the end-to-end flow

1. Launch backend in Colab and copy `PUBLIC URL`.
2. Edit `index.html` → set `BACKEND_URL`.
3. Open `index.html` in browser.
4. Upload video and choose a mode.
5. Wait until frontend polls `/check` and shows the results.

---

## 🛠️ Troubleshooting & common fixes

* **Results not shown** → Confirm `/content/output/summary_fixed.mp4` exists.
* **`/check` returns HTML** → Ensure polling the correct backend URL.
* **CORS errors** → Use HTTPS ngrok + open frontend via local server (`python -m http.server`).
* **ngrok URL changes** → Update `BACKEND_URL` after restart.

---

## 🔒 Security & production notes

* This setup is for testing/dev. Do not expose ngrok URL publicly.
* Add authentication in production.
* Restrict file sizes/types.

---

## 📦 Helpful git commands for GitHub

```bash
# initialize repo
git init
git add .
git commit -m "Initial commit"

# add remote and push
git remote add origin git@github.com:yourname/your-repo.git
git branch -M main
git push -u origin main
```

**Recommended `.gitignore`:**

```
__pycache__/
*.pyc
*.pyo
*.pyd
*.ipynb_checkpoints/
.DS_Store
content/
*.mp4
*.avi
*.mov
node_modules/
dist/
.env
```

---

## ✨ Next steps & ideas

* Add authentication to `/upload`.
* Support multiple jobs with job IDs.
* Show live notebook logs in frontend.
* Add webhook/email notifications.
* Dockerize backend for deployment.

---

## 📄 License

MIT or any license of your choice.

---

### 🥳 Final note

You now have a full E2E system with frontend, backend, and multiple notebooks. Update small placeholders (`BACKEND_URL`, notebook names) and push this README to GitHub.
