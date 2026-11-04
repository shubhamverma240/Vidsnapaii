# 🎬 VidSnap AI

**Turn images and a text description into an AI-voiced vertical reel, automatically.**

VidSnap AI is a full stack web application that automates short-form video creation. Upload a few images, write a short description, and the app generates an AI voiceover and combines everything into a **1080×1920, 30 FPS vertical reel**, ready for Instagram Reels, YouTube Shorts, or TikTok.

A Flask web server handles uploads, while a separate background worker handles the audio and video processing, so the website stays fast and responsive.

## 🎥 Demo

A sample output is included in this repo: [`reel.mp4`](reel.mp4)

## ✨ Features

- Upload multiple images along with a text description
- AI voiceover using the ElevenLabs Text-to-Speech API
- Automatic video creation with FFmpeg (scaling, padding and encoding to 1080×1920 at 30 FPS)
- Background worker that picks up new uploads and converts them automatically
- UUID-based folders for every request, so files never conflict
- Duplicate-processing prevention, so no reel is generated twice
- Gallery page to browse and play all generated reels

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| Backend | Python, Flask |
| Frontend | HTML, CSS, Jinja2 |
| AI / API | ElevenLabs Text-to-Speech |
| Video & Audio | FFmpeg |

## 📁 Project Structure

```
├── main.py                # Flask app (home, create, gallery routes)
├── generate_process.py    # Background worker: audio + video generation
├── text_to_audio.py       # ElevenLabs text-to-speech helper
├── config.py              # API key configuration
├── templates/             # HTML pages
├── static/css/            # Stylesheets
└── reel.mp4               # Sample output
```

## 🚀 Getting Started

### Prerequisites

- Python 3.9 or higher
- [FFmpeg](https://ffmpeg.org/download.html) installed and added to your system PATH
- An [ElevenLabs](https://elevenlabs.io) API key (free plan works)

### Installation

**1. Clone the repository**

```bash
git clone https://github.com/shubhamverma240/VidSnapAI.git
cd VidSnapAI
```

**2. Install dependencies**

```bash
pip install flask elevenlabs
```

**3. Add your API key**

Open `config.py` and replace the placeholder with your own key:

```python
ELEVENLABS_API_KEY = "your_api_key_here"
```

**4. Create the required folders and file**

```bash
mkdir user_uploads
mkdir static/reels
```

Then create an empty file named `done.txt` in the project folder.

**5. Run the app** (use two terminals)

```bash
# Terminal 1: web server
python main.py

# Terminal 2: background worker
python generate_process.py
```

Open **http://127.0.0.1:5000** in your browser.

## 🧭 How to Use

1. Open the **Create Reel** page.
2. Upload your images and enter the text you want narrated.
3. Submit and wait a few seconds while the reel is generated.
4. Open the **Gallery** page to watch it.

## 🔮 Future Improvements

- Background music selection
- Custom duration and transitions for each image
- Voice and language selection
- Progress indicator while a reel is being generated
- Cloud deployment

## 👤 Author

**Shubham Verma**
Computer Science Engineering graduate | Python · Flask · React · SQL

[LinkedIn](https://www.linkedin.com/in/shubham-verma-624b5824a) · [GitHub](https://github.com/shubhamverma240)
