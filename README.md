# IntelliDrive: Adaptive Vision-Based Driver Drowsiness Detection System Using Real-Time Eye Blink Analysis

> An AI-powered safety system that monitors driver alertness in real time using computer vision and deep learning, triggering an audio alarm before fatigue leads to accidents.

---

## How It Works

IntelliDrive uses your webcam to continuously monitor a driver's face. It combines two detection strategies for maximum reliability:

1. **Haar Cascade Eye Tracking** — Detects whether eyes are open or closed frame-by-frame and measures how long they remain shut
2. **Teachable Machine CNN Model** — A trained Keras/TensorFlow model classifies the driver's face region as `drowsy` or `non drowsy`

If either signal crosses the drowsiness threshold, an audio alarm fires immediately.

---

## Features

| Feature | Description |
|---|---|
|  **Personalized Calibration** | 30-second startup phase learns your natural blink pattern to set a personalized drowsiness threshold |
|  **Dual Detection** | Combines eye-closure timing + CNN model prediction for robust, low-false-alarm detection |
|  **Blink Analytics** | Tracks blink count, blink rate (per minute), and continuous eye-closure duration live |
|  **Audio Alarm** | Plays a looping alarm (`alarm.wav`) the moment drowsiness is detected; stops when driver recovers |
|  **Live HUD Overlay** | Real-time dashboard on the video feed — face status, eye state, blink rate, closed time, mode, and threshold |
|  **Recalibrate Anytime** | Press `R` to restart calibration without restarting the app |

---

## Project Structure

```
intellidrive/
├── app.py              # Main entry point — orchestrates camera, model, alarm, and UI
├── camera.py           # Webcam capture + Haar Cascade face & eye detection
├── model.py            # TensorFlow/Keras model loader and inference wrapper
├── keras_model.h5      # Trained Teachable Machine CNN (224×224 input)
├── labels.txt          # Class labels: 0 drowsy / 1 non drowsy
└── alarm.wav           # Audio alert played on drowsiness detection
```

---

## Installation

### Prerequisites

- Python 3.8+
- A working webcam

### 1. Clone the repository

```bash
git clone https://github.com/your-username/intellidrive.git
cd intellidrive
```

### 2. Install dependencies

```bash
pip install opencv-python tensorflow pygame numpy
```

### 3. Run

```bash
python app.py
```

---

## Controls

| Key | Action |
|-----|--------|
| `R` | Recalibrate — resets threshold to your current blink pattern |
| `Q` | Quit the application |

---

## Detection Logic

### Calibration Phase (first 30 seconds)
The system records your natural blink durations. Once complete, it computes:

```
personalized_threshold = 6 × average_blink_duration
```

This means if your average blink is 0.15s, the drowsiness threshold is set to ~0.9s of continuous eye closure — tuned to *you*, not a generic default.

### Drowsiness Triggers
The alarm fires if **either** condition is met (after calibration):
- Eye closure duration ≥ personalized threshold
- CNN model predicts `DROWSY` with majority confidence

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Vision | OpenCV (Haar Cascades) |
| Deep Learning | TensorFlow / Keras |
| Model Training | Google Teachable Machine |
| Audio | Pygame mixer |
| Language | Python 3 |

---

## Model Details

- **Architecture**: MobileNet-based transfer learning (Teachable Machine export)
- **Input size**: 224 × 224 RGB
- **Preprocessing**: Normalized to `[-1, 1]`
- **Classes**: `drowsy`, `non drowsy`
- **Format**: Keras `.h5`

---

## Possible Improvements

- [ ] Replace Haar Cascades with a landmark-based eye aspect ratio (EAR) detector for higher accuracy
- [ ] Add yawning detection as a third drowsiness signal
- [ ] Log drowsiness events with timestamps to a CSV
- [ ] Package as a standalone desktop app with PyInstaller
- [ ] Add a sensitivity/threshold slider in the UI

---

## Disclaimer

This project is a **prototype for educational and research purposes**. It should not be solely relied upon as a safety-critical system in real driving environments.

---

## License

MIT License — feel free to use, modify, and distribute with attribution.
