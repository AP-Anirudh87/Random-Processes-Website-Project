# Complex Random Process Analysis for Communication Systems with Applications to Speech Enhancement

[![Python](https://img.shields.io/badge/Python-3.10%20%7C%203.11%20%7C%203.12%20%7C%203.13-blue.svg)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/Framework-Flask%203.x-green.svg)](https://flask.palletsprojects.com/)
[![DSP](https://img.shields.io/badge/DSP-SciPy%20%26%20NumPy-orange.svg)](https://scipy.org/)
[![Database](https://img.shields.io/badge/Database-SQLite%20%2B%20SQLAlchemy-lightgrey.svg)](https://www.sqlite.org/)
[![SDGs](https://img.shields.io/badge/UN%20SDGs-3%20%7C%204%20%7C%209%20%7C%2010-purple.svg)](https://sdgs.un.org/goals)

---

## 📌 Academic Metadata
* **Project Title:** Complex Random Process Analysis for Communication Systems with Applications to Speech Enhancement
* **Author:** A.P. Anirudh
* **Registration Number:** `24BEC1158`
* **Faculty Guide:** Dr. Kalaivan K
* **Institution:** Vellore Institute of Technology (VIT)
* **Department:** School of Electronics Engineering (SENSE)

---

## 📖 Executive Summary
In telecommunication channels, signals transmitted across physical media (e.g., copper, radio, optical, and VoIP links) are inherently degraded by stochastic noise phenomena. The dominant mathematical model for this impairment is **Additive White Gaussian Noise (AWGN)**, a wide-band random process with a flat power spectral density ($S_N(f) = \frac{N_0}{2}$) and Gaussian amplitude probability density function.

This project implements an end-to-end mathematical analysis and interactive full-stack web application designed to:
1. **Model & Synthesize Random Processes:** Generate calibrated channel noise $n(t)$ at precise Signal-to-Noise Ratios (SNR in dB) and inject it into modulated voice/test signals $s(t)$.
2. **Apply Digital Signal Processing (DSP) Filtration:** Utilize a **4th-order Butterworth Low-Pass Digital Filter** ($N=4$, $f_c=4000\text{ Hz}$, implemented via Second-Order Sections `sosfiltfilt` for zero phase distortion) to recover the signal envelope $y(t)$.
3. **Quantify Performance Metrics:** Empirically compute SNR improvement ($\Delta\text{SNR}$) and Mean Square Error ($\text{MSE}$) between $s(t)$ and $y(t)$.
4. **Dual Automatic Speech Recognition (ASR):** Run real-time speech recognition on both the noisy corrupted signal $x(t)$ and the enhanced recovered signal $y(t)$ to measure perceptual intelligibility gains.
5. **AI Semantic Distillation:** Provide automated multi-lingual summarization of the extracted intelligence (English, Hindi, Tamil).

---

## 🎯 UN Sustainable Development Goals (SDGs)
* **SDG 3 — Good Health and Well-being:** Enhances speech intelligibility for telemedicine and emergency remote clinical consultations over low-bandwidth or noisy connections.
* **SDG 4 — Quality Education:** Cleanses voice audio for remote lectures, assistive classroom technologies, and accessible e-learning portals in developing regions.
* **SDG 9 — Industry, Innovation, and Infrastructure:** Delivers robust DSP noise-mitigation algorithms for IoT telecommunication nodes and next-generation voice networks.
* **SDG 10 — Reduced Inequalities:** Bridges communication divides by providing accessible, lightweight audio enhancement across multiple regional languages.

---

## 🔬 Mathematical Framework

```
Transmitter                  Noisy Channel                         Receiver & DSP Engine
 ┌─────────┐                ┌─────────────┐                      ┌─────────────────────────┐
 │ Pure    │  s(t)          │ AWGN Channel│      x(t)=s(t)+n(t)  │ 4th-Order Butterworth   │  y(t)
 │ Signal  ├───────────────►│  + n(t)     ├─────────────────────►│ Digital Low-Pass Filter ├────────► Dual ASR &
 └─────────┘                └─────────────┘                      └─────────────────────────┘         AI Summary
                               ▲                                              │
                               │                                              ▼
                        Stochastic Noise                            Metrics: SNR & MSE
```

### 1. Transmitted Modulated Signal $s(t)$
$$s(t) = A_c \left[1 + \mu \cdot m(t)\right] \cos(2\pi f_c t)$$
where $\mu$ is the modulation index ($0.85$), $f_c$ is the carrier frequency, and $m(t)$ represents the message baseband signal.

### 2. Stochastic Noise Model $n(t)$
Additive White Gaussian Noise (AWGN) with zero mean ($\mu_n = 0$) and calibrated variance $\sigma_n^2$:
$$P_s = \frac{1}{N}\sum_{k=0}^{N-1} s[k]^2, \quad \sigma_n = \sqrt{P_s \cdot 10^{-\text{SNR}_{\text{target}} / 10}}$$
$$n[k] \sim \mathcal{N}(0, \sigma_n^2)$$

### 3. Butterworth Filter Transfer Function
The magnitude response of an $N$-th order Butterworth low-pass filter with cutoff frequency $\omega_c$:
$$|H(j\omega)|^2 = \frac{1}{1 + \left(\frac{\omega}{\omega_c}\right)^{2N}}$$
Implemented digitally using **Second-Order Sections (SOS)** to minimize round-off noise and numerical instability.

### 4. Evaluation Metrics
* **Signal-to-Noise Ratio (SNR):**
  $$\text{SNR}_{\text{dB}} = 10 \log_{10} \left( \frac{\sum_{k=0}^{N-1} s[k]^2}{\sum_{k=0}^{N-1} \left(s[k] - \hat{s}[k]\right)^2} \right)$$
* **Mean Square Error (MSE):**
  $$\text{MSE} = \frac{1}{N} \sum_{k=0}^{N-1} \left( s[k] - y[k] \right)^2$$
* **SNR Gain:**
  $$\Delta\text{SNR} = \text{SNR}_{\text{output}} - \text{SNR}_{\text{input}}$$

---

## 📁 Repository Structure

```
RP Project/
│
├── app.py                  # Main Flask application server & RESTful API endpoints
├── speech_engine.py        # Core DSP pipeline, Butterworth LPF, AWGN model, ASR & AI summarizer
├── database.py             # SQLite ORM models, session management, and metric tracking
├── requirements.txt        # Python dependency manifest
├── pyrefly.toml            # IDE language server configuration
├── pyrightconfig.json      # Pyright type checker search paths
├── .gitignore              # Git ignore file for Python, audio uploads, and databases
│
├── templates/
│   └── index.html          # Interactive Single-Page Application (SPA) UI
│
├── static/
│   ├── css/
│   │   └── style.css       # Responsive Glassmorphism UI styling
│   └── js/
│       └── main.js         # Frontend controller, Chart.js visualizer, audio player
│
├── uploads/                # Temporary directory for uploaded input audio (auto-generated)
├── processed/              # Filtered and enhanced audio output files (auto-generated)
└── instance/               # Local SQLite database `dsp_app.db` (auto-generated)
```

---

## 🚀 How to Run the Application

### Option 1: Running on Your Local Computer

#### Prerequisites
* **Python 3.10 to 3.13** installed on your system.
* (Optional) **FFmpeg** installed if processing non-WAV audio files (e.g., MP3/M4A).

---

#### 💻 Windows (PowerShell / Command Prompt)

1. **Open PowerShell / Terminal** and navigate into the `RP Project` folder:
   ```powershell
   cd ".......\..FOLDER..\.......\RP Project"
   ```

2. **Create a Python Virtual Environment:**
   ```powershell
   python -m venv venv
   ```
   *(If `python` is not in your PATH, use your full executable path, e.g., `& "C:\Users\<YourUser>\AppData\Local\Programs\Python\Python313\python.exe" -m venv venv`)*

3. **Activate the Virtual Environment:**
   ```powershell
   .\venv\Scripts\Activate.ps1
   ```
   *(If prompted with an execution policy error, run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` first).*

4. **Install Required Packages:**
   ```powershell
   pip install -r requirements.txt
   ```

5. **Start the Flask Server:**
   ```powershell
   python app.py
   ```

6. **Open in Web Browser:**
   Navigate to: **[http://127.0.0.1:5000](http://127.0.0.1:5000)**

---

#### 🍎 macOS / 🐧 Linux / WSL (Bash)

1. **Navigate to the Project Directory:**
   ```bash
   cd "RP Project"
   ```

2. **Create and Activate Virtual Environment:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run Application:**
   ```bash
   python3 app.py
   ```

5. **Open Browser:**
   Visit **[http://localhost:5000](http://localhost:5000)**

---

### Option 2: Running Online in GitHub (GitHub Codespaces / Cloud Bash)

You can run this full-stack application directly in your web browser without installing anything on your computer using **GitHub Codespaces**.

1. **Open Your GitHub Repository** in your browser.
2. Click the green **`<> Code`** button.
3. Select the **`Codespaces`** tab and click **`Create codespace on main`**.
4. In the built-in terminal (at the bottom of the Codespaces screen), execute these commands:

```bash
# 1. Navigate to project folder (if at root)
cd "Random-Processes-Website-Project"

# 2. Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch the application
python3 app.py
```

5. When Flask starts on port `5000`, a notification popup will appear at the bottom right:
   > **"Your application running on port 5000 is available."**
   
   Click **`Open in Browser`** to access the live app directly in your browser.

---

## 🌐 RESTful API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Serves the interactive SPA frontend |
| `GET` | `/api/health` | Service health, version, and author metadata |
| `POST` | `/api/upload` | Upload audio file (`wav`, `mp3`, `flac`, `ogg`) |
| `POST` | `/api/generate-sample` | Generate synthetic AWGN test sample (e.g. `cafe_noise`, `traffic_noise`) |
| `POST` | `/api/process/<session_id>` | Execute Butterworth filtering, SNR/MSE metrics, and dual ASR |
| `GET` | `/api/results/<session_id>` | Retrieve session results and Chart.js waveform arrays |
| `POST` | `/api/re-summarize/<session_id>` | Instant re-summarization across languages (`en-US`, `hi-IN`, `ta-IN`) |
| `GET` | `/api/audio/<type>/<session_id>` | Stream audio for comparison playback (`type` = `input` or `output`) |
| `GET` | `/api/download-report/<session_id>` | Download a plain-text mathematical analysis report |
| `GET` | `/api/project-info` | Project metadata, author information, and SDG mapping |

---

## 🛠️ Technology Stack
* **Backend:** Python 3.10–3.13, Flask 3.x, Flask-SQLAlchemy, Werkzeug
* **Signal Processing:** SciPy (`scipy.signal.butter`, `sosfiltfilt`), NumPy
* **Audio Engineering:** PyDub, Wave, Struct, Audioop-lts
* **Speech Recognition:** SpeechRecognition (Google Web Speech API integration)
* **Frontend:** HTML5, Modern Vanilla CSS (Glassmorphism design tokens), Vanilla JavaScript (ES6+)
* **Visualization:** Chart.js for real-time comparative waveform rendering
* **Database:** SQLite 3

---

## 👨‍💻 Author & Acknowledgements
* **Author:** A.P. Anirudh (`24BEC1158`)
* **Faculty Guide:** Dr. Kalaivan K
* **Institution:** Vellore Institute of Technology (VIT), Chennai Campus
* Developed as part of the coursework for **Complex Random Process Analysis for Communication Systems**.