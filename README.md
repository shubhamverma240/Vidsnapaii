🎬 VidSnap AI
Turn images and a text description into an AI-voiced vertical reel, automatically.

Show Image Show Image Show Image Show Image Show Image

</div>
📖 Overview

VidSnap AI is a full stack web application that automates short-form video creation. A user uploads a few images and writes a short description. The app generates a natural-sounding AI voiceover from the text, combines it with the images, and delivers a 1080×1920, 30 FPS vertical reel ready for Instagram Reels, YouTube Shorts, or TikTok.

It uses a two-process architecture: a Flask web server handles uploads, and a separate background worker does the heavy audio and video processing, so the website stays fast and responsive.

🎥 Demo

A sample output is included in the repo: reel.mp4

<!-- Optional: add screenshots after uploading them to an images/ folder ![Home Page](images/home.png) ![Create Page](images/create.png) ![Gallery](images/gallery.png) -->
✨ Features
📤 Multi-image upload with a text description for the voiceover
🗣️ AI voiceover using the ElevenLabs Text-to-Speech API (eleven_turbo_v2_5 model)
🎞️ Automatic video creation with FFmpeg: scaling, padding and encoding to 1080×1920 at 30 FPS
⚙️ Background processing: a worker continuously watches for new uploads and converts them
🔑 UUID-based file management: each request gets its own folder, so there are no file conflicts
🚫 Duplicate-processing prevention: finished jobs are tracked, so nothing is processed twice
🖼️ Reel gallery to browse and play all generated reels
🛠️ Tech Stack
Layer	Technology
Backend	Python, Flask, Werkzeug
Frontend	HTML, CSS, Jinja2 templates
AI / API	ElevenLabs Text-to-Speech
Video & Audio	FFmpeg
File handling	UUID, os, subprocess
🏗️ Architecture
 ┌──────────────┐    upload     ┌─────────────────────┐
 │   Browser    │ ────────────▶ │  Flask app (main.py)│
 └──────────────┘               └──────────┬──────────┘
        ▲                                  │ saves images + desc.txt + input.txt
        │                                  ▼
        │                       user_uploads/<uuid>/
        │                                  │
        │                                  ▼  polls every 4s
        │                    ┌──────────────────────────────┐
        │                    │ Worker (generate_process.py) │
        │                    │  1. desc.txt → audio.mp3     │ ← ElevenLabs API
        │                    │  2. images + audio → reel    │ ← FFmpeg
        │                    └──────────────┬───────────────┘
        │                                   ▼
        └──────────────────────── static/reels/<uuid>.mp4
                                  (shown in the Gallery)
📁 Project Structure
vidsnap-ai/
├── main.py                 # Flask app: routes for /, /create, /gallery
├── generate_process.py     # Background worker: audio + video generation
├── text_to_audio.py        # ElevenLabs text-to-speech helper
├── config.py               # API key configuration (add your own key)
├── templates/              # HTML pages (base, index, create, gallery)
├── static/
│   └── css/                # Stylesheets
├── reel.mp4                # Sample output
└── README.md

user_uploads/ and static/reels/ are created at runtime to store uploads and generated videos.

🚀 Getting Started
Prerequisites
Python 3.9+
FFmpeg installed and available in your system PATH (download) Check with: ffmpeg -version
A free ElevenLabs API key (get one here)
1. Clone the repository
bash
git clone https://github.com/shubhamverma240/VidSnapAI.git
cd VidSnapAI
2. Install dependencies
bash
pip install flask elevenlabs
3. Add your API key

Open config.py and replace the placeholder with your own key:

python
ELEVENLABS_API_KEY = "your_api_key_here"

⚠️ Never commit your real API key. Keep it on your machine only.

4. Create the required folders and files
bash
mkdir user_uploads
mkdir static/reels

Then create an empty file named done.txt in the project root.

5. Run the app (two terminals)

Terminal 1: web server

bash
python main.py

Terminal 2: background worker

bash
python generate_process.py

Open http://127.0.0.1:5000 in your browser.

🧭 How to Use
Go to Create Reel.
Upload your images and write the text you want narrated.
Submit and wait a few seconds while the worker generates your reel.
Open the Gallery to watch it.
🔍 How It Works (Under the Hood)
Upload: Flask saves each image to user_uploads/<uuid>/, writes the description to desc.txt, and builds an FFmpeg input.txt listing every image with its duration.
Voiceover: the worker reads desc.txt and calls the ElevenLabs API to produce audio.mp3.
Video: FFmpeg concatenates the images, scales and pads them to 1080×1920, adds the audio, and encodes to H.264/AAC at 30 FPS:
bash
   ffmpeg -f concat -safe 0 -i input.txt -i audio.mp3 \
     -vf "scale=1080:1920:force_original_aspect_ratio=decrease,pad=1080:1920:(ow-iw)/2:(oh-ih)/2:black" \
     -c:v libx264 -c:a aac -shortest -r 30 -pix_fmt yuv420p output.mp4
Tracking: the finished folder name is saved to done.txt so it is never processed twice.
📚 What I Learned
Building a full stack app with Flask, including routing, file uploads and templates
Integrating a third-party AI API (ElevenLabs) and handling secrets safely
Automating video and audio processing with FFmpeg
Designing a background worker that is separate from the web server
Managing unique user data with UUID-based folders
🔮 Future Improvements
 Background music selection
 Custom duration and transitions per image
 Voice and language selection
 Live progress/status indicator on the Create page
 Replace the polling worker with a task queue (Celery + Redis)
 Cloud deployment (Render / Railway)
🔐 Security Note

API keys must never be committed. This repository ships config.py with a placeholder only. Add your own key locally and do not push it.

👤 Author

Shubham Verma Computer Science Engineering graduate | Python · Flask · React · SQL
