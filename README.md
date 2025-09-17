# Multimodal_Video_Summarization_Final_Year_Project_2025
This Repository Contains all the files related to Multimodal_Video_Summarization_Final_Year_Project_2025


MultiModal Video Summarization 🎬✨

Single-file frontend + Colab/Flask backend that runs one of three notebooks depending on user choice:

Full Video Summarization (mode=full)

Time-Stamp Based Summarization (mode=timestamp)

Presenter-Image Based Summarization (mode=presenter)

This README is copy-paste ready for your GitHub repo — clear steps, commands, examples and troubleshooting. Use it as README.md.

Table of contents

What is this

Features 🚀

Repository structure 📁

Prerequisites ⚙️

Backend (Google Colab) — Quick start 🧪

Frontend — index.html 🔗

How modes & file paths map 🔄

Testing the end-to-end flow ✅

Troubleshooting & common fixes 🛠️

Security & production notes 🔒

Helpful git commands for GitHub 📦

Next steps & ideas ✨

License 📄

What is this ❓

This project provides a simple, attractive frontend (index.html) that uploads a test.mp4 video and triggers server-side Colab notebooks via a small Flask server running in Google Colab (exposed with ngrok). The backend chooses one of three notebooks based on the mode parameter and returns a downloadable summary video + text.

Features ✅

Single-file frontend with polished UI and animations.

Upload test.mp4 and choose between:

Full summarization

TimeStamp based summarization (upload time.txt)

Presenter image based summarization (upload image)

Backend runs the corresponding Colab notebook and re-encodes outputs for browser download.

Polling (/check) to update frontend when results are ready.

Outputs served at /output/summary_fixed.mp4 and /output/summary.txt.

Repository structure 📁 (recommended)
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

Prerequisites ⚙️

Google account (for Colab).

ngrok authtoken (optional if using pyngrok from Colab which can request it).

Modern browser for frontend.

The frontend BACKEND_URL must be set to the ngrok public URL printed by the Colab backend (no trailing slash).

Backend (Google Colab) — Quick start 🧪

Open a new Colab and paste the provided backend server code into a cell (the Flask + pyngrok script).

Make sure notebook paths at the top match your actual notebook filenames:

NOTEBOOK_FULL      = '/content/Full_Video_Summarization_final.ipynb'
NOTEBOOK_TIMESTAMP = '/content/Time_Stamp_Based_Final_.ipynb'
NOTEBOOK_PRESENTER = '/content/Presenter_Image_Based_Summarization.ipynb'


Run the cell. It installs dependencies, starts Flask and opens an ngrok URL. Colab prints something like:

🚀 PUBLIC URL: https://abcd-1234.ngrok-free.app


Copy that URL and set in your frontend index.html as BACKEND_URL.

Important Colab file paths (canonical):

Input video saved by server: /content/input/test.mp4

Timestamp file (timestamp mode): /content/input/time.txt

Presenter image (presenter mode): /content/input/presenter_image.<ext>

Raw outputs produced by notebooks:

Full: /content/summary_video.mp4

Timestamp: /content/timestamp_requested.mp4

Presenter: /content/presenter_requested.mp4

Final re-encoded output (what frontend uses): /content/output/summary_fixed.mp4

Final text summary: /content/output/summary.txt

Frontend — index.html 🔗

Replace BACKEND_URL near the top of index.html with your ngrok URL, e.g.

const BACKEND_URL = 'https://abcd-1234.ngrok-free.app';


Open index.html in your browser (double-click or serve via a static server). Upload test.mp4, choose a mode and upload any extra file required (time.txt or image), then click the appropriate button.

How modes & file paths map 🔄

mode=full

Notebook: NOTEBOOK_FULL

Notebook expected to produce: /content/summary_video.mp4 (or directly write /content/output/summary_fixed.mp4 & /content/output/summary.txt)

mode=timestamp

Notebook: NOTEBOOK_TIMESTAMP

Input file: /content/input/time.txt

Notebook expected to produce: /content/timestamp_requested.mp4

mode=presenter

Notebook: NOTEBOOK_PRESENTER

Input file: /content/input/presenter_image.<ext>

Notebook expected to produce: /content/presenter_requested.mp4

Backend behavior: after the selected notebook finishes, backend re-encodes the raw file for the browser and writes /content/output/summary_fixed.mp4, then /check becomes ready: true.

Testing the end-to-end flow ✅

Launch the Colab backend and copy the PUBLIC URL.

Edit index.html and set BACKEND_URL to that URL (no trailing slash).

Open index.html in browser.

Upload test.mp4.

For Full Video:

Click Full Video Summarization. Wait for the 15:00 UI timeout or until the backend finishes. Frontend polls /check every 10s and will show the summary video + text when ready.

For TimeStamp:

Click User Request Based Summarization → Time Stamp Based. Upload time.txt. Click Get Video (5:00). Wait.

For Presenter:

Click Presenter Images Based. Upload presenter image (PNG/JPG). Click Get Video (10:00). Wait.

Once done, you will see summary text and a link + embedded player for summary_fixed.mp4.

Troubleshooting & common fixes 🛠️

Frontend never shows results but backend has outputs

Confirm /content/output/summary_fixed.mp4 exists in Colab. Backend only signals ready when this exact file exists.

Check Colab logs: the backend prints which raw file it expected and whether re-encode ran.

/check returns HTML instead of JSON

Make sure you're polling the correct backend URL (/check) and not the Colab notebook UI. Check console network tab for response body.

CORS / Mixed content errors

Use https ngrok URL and open frontend with file:// sometimes causes issues. Open a simple local HTTP server (python -m http.server) and use http://localhost:8000/index.html if needed.

ngrok URL changed after restarting Colab

Update BACKEND_URL in index.html each time you restart the Colab backend.

Notebook takes longer than UI timeout

Increase --ExecutePreprocessor.timeout in backend run_notebook() or increase frontend timer.

Raw output has different name

Edit the backend constant RAW_SUMMARY_* to match your notebook outputs.

Security & production notes 🔒

This setup is for testing/development. Do not expose ngrok URL publicly for production use.

Add authentication (API key / token) before accepting uploads in production. I can add that if required.

Limit upload size and file types to avoid abuse. Backend currently checks for file presence but not heavy validation.

Helpful git commands for GitHub 📦
# initialize repo (run in project root)
git init
git add .
git commit -m "Initial commit - multimodal video summarization frontend + backend"
# create remote and push (replace URL)
git remote add origin git@github.com:yourname/your-repo.git
git branch -M main
git push -u origin main

Recommended .gitignore
# Python / Notebook
__pycache__/
*.pyc
*.pyo
*.pyd
*.ipynb_checkpoints/
# OS
.DS_Store
# Colab output and media large files (do not push big videos)
content/
*.mp4
*.avi
*.mov
# node / build
node_modules/
dist/
.env

Next steps & ideas ✨

Add authentication to /upload.

Save per-job directories and allow multiple jobs (job IDs).

Show live notebook logs in frontend.

Add webhook/email notification on completion.

Dockerize the backend for deployment.

License 📄

Use whatever license you prefer. If you want, I can add an MIT license file for you.
