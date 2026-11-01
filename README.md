# AI Gym Trainer — Real-Time Exercise Form Assistant

**Maintained by Avdhoot Ukirde**

An independent, Streamlit-based AI fitness application that uses webcam video and MediaPipe Pose Landmarker to estimate exercise movement, count repetitions, display form indicators, and record workout summaries. Optional Groq-powered coaching and text-to-speech provide additional feedback.

> This repository is an independent GitHub repository, not a GitHub fork. Some inherited implementation and assets are adapted from [shradha-khapra/ai-gym-coach](https://github.com/shradha-khapra/ai-gym-coach). Please review the upstream project's licensing and attribution requirements before redistributing inherited material. No upstream license file was present in the source repository when this project was transferred; this repository does not claim ownership of third-party material.

## Features

- Live webcam input through Streamlit WebRTC.
- Human pose estimation with MediaPipe Pose Landmarker.
- Exercise-specific repetition tracking for:
  - Squats
  - Push-ups
  - Dumbbell biceps curls
  - Shoulder press
  - Lunges
- Pose skeleton and exercise feedback overlays.
- Workout plans with target sets and repetitions.
- Per-profile workout history stored in SQLite.
- Optional Groq LLM coaching with gTTS voice output.
- Separate responsive HTML/CSS landing page.

The form indicators are heuristic estimates based on 2D pose landmarks. They are not medical assessments, a substitute for a qualified trainer, or a guarantee that an exercise is safe or correctly performed.

## Technology stack

| Area | Technology |
|---|---|
| Language | Python |
| Application UI | Streamlit |
| Real-time video | streamlit-webrtc, OpenCV, PyAV |
| Pose estimation | MediaPipe Tasks — Pose Landmarker |
| Numerical processing | NumPy |
| Workout history | SQLite |
| Optional AI coaching | Groq API |
| Text-to-speech | gTTS |
| Landing page | HTML5, CSS3 |

## Repository structure

```text
AI-Gym-Trainer-Final-Year-Project-/
├── LandingPage/
│   ├── index.html
│   ├── style.css
│   └── fonts/
├── Main App/
│   ├── main.py
│   ├── requirements.txt
│   ├── packages.txt
│   ├── .env.example
│   ├── core/
│   ├── detectors/
│   ├── ml_models/          # model downloads here on first startup
│   ├── services/
│   ├── static/
│   └── tutorial-info/
├── .gitignore
└── README.md
```

## Requirements

- Python **3.11** is the recommended environment for this dependency set.
- A webcam or camera accessible to the browser.
- A modern browser with camera permissions enabled.
- Internet access for package installation and the first model download.
- A Groq API key only if you want optional AI-generated coaching.

The MediaPipe Pose Landmarker model is downloaded automatically on first startup from Google's version-pinned model storage URL and saved to `Main App/ml_models/pose_landmarker_full.task`. The application checks the downloaded file size before loading it. The model is approximately 9.4 MB, so the first launch requires an internet connection. The model is distributed under Apache 2.0 according to the MediaPipe project maintainer's clarification; see the [MediaPipe model documentation](https://developers.google.com/edge/mediapipe/solutions/vision/pose_landmarker).

## Quick start — Windows

Open PowerShell in the repository folder:

```powershell
cd "Main App"
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
Copy-Item .env.example .env
streamlit run main.py
```

Open the local URL printed by Streamlit (normally `http://localhost:8501`) and allow camera access when your browser asks.

## Quick start — macOS / Linux

```bash
cd "Main App"
python3.11 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
cp .env.example .env
streamlit run main.py
```

## Optional Groq coaching

The workout tracking features are designed to run without a Groq API key. To enable AI-generated coaching, add your key to `Main App/.env`:

```dotenv
GROQ_API_KEY=your_groq_api_key_here
```

Never commit a real `.env` file, API key, or Streamlit secrets file. For Streamlit Community Cloud, add `GROQ_API_KEY` in the app's Secrets settings instead. gTTS also requires internet access.

## How to use

1. Launch the Streamlit application.
2. Enter a username to create or reopen a local workout profile.
3. Choose an exercise and set target sets and repetitions.
4. Start the workout and allow browser camera access.
5. Position your full body in view, with adequate lighting and relevant joints visible.
6. Follow the repetition count and on-screen form indicators.
7. End the workout and review the saved summary in Workout History.

The username screen is a lightweight local profile selector, not secure account authentication. Workout history is stored in `Main App/data.db`; the database is excluded from Git.

## Deploy on Streamlit Community Cloud

1. Create a Streamlit Community Cloud app from this repository.
2. Select the `main` branch.
3. Set **Main file path** to `Main App/main.py`.
4. Select Python 3.11 if the option is available.
5. Add `GROQ_API_KEY` to Streamlit Secrets if optional AI coaching is required.
6. Deploy and review the app logs if installation or startup fails.

The app requires camera permission and a secure browser context (HTTPS) for remote camera use. Confirm that your hosting provider supports WebRTC. SQLite storage on hosted services may be ephemeral, so workout history may not survive restarts or redeployments.

## Troubleshooting

| Problem | What to check |
|---|---|
| First launch fails while loading the model | Confirm internet access; the app downloads the model automatically on first startup. |
| Camera does not start | Use localhost or HTTPS, grant camera permission, and check browser/host WebRTC support. |
| No pose is detected | Improve lighting, move farther from the camera, and keep the required body joints visible. |
| AI coaching is unavailable | Confirm `GROQ_API_KEY` and outbound network access. |
| Spoken feedback is unavailable | Check internet connectivity; gTTS uses an external service. |
| Package installation fails | Use Python 3.11 and recreate the virtual environment. |

## Landing page

Open `LandingPage/index.html` in a browser. This is a static project introduction; the workout application runs separately through Streamlit.

## Safety and privacy

- Exercise feedback is an approximate computer-vision aid and can be wrong when landmarks are occluded, lighting is poor, or camera placement is unsuitable.
- Stop exercising if you experience pain, dizziness, or unusual discomfort. Seek guidance from a qualified professional when needed.
- Live frames are processed by the application for pose estimation. Review your hosting and WebRTC configuration before using sensitive footage.
- Do not store API keys or private credentials in Git.

## Maintainer

**Avdhoot Ukirde**  
GitHub: [@Avdhoot-Ukirde](https://github.com/Avdhoot-Ukirde)  
Email: [avdhootukirde106@gmail.com](mailto:avdhootukirde106@gmail.com)
