# 🎵 Audio LED Bar

A real-time audio-reactive LED bar built with **Arduino Uno** and a **Python GUI** — LEDs rise and fall with the beat of your microphone input, just like a studio VU meter.

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python&logoColor=white)
![Arduino](https://img.shields.io/badge/Arduino-Uno-teal?logo=arduino&logoColor=white)
![PyQt5](https://img.shields.io/badge/GUI-PyQt5-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## ✨ Features

- 🎚️ **32-band FFT equalizer** visualized in a dark-themed GUI
- 🎙️ Live microphone input — works with any system audio device
- 💡 **18 LEDs** driven by Arduino, color-graded Green → Yellow → Red
- 🔊 Smart **noise gate** + auto noise-floor tracking (no flickering in silence)
- 🎛️ Real-time sensitivity slider
- 🔌 Auto-detect Arduino COM port from the GUI — no code editing needed
- 👁️ Preview mode (works without Arduino connected)

---

## 🛒 Components

| Component | Quantity |
|---|---|
| Arduino Uno | 1 |
| Blue LEDs | 6 |
| Green LEDs | 5 |
| White LEDs | 3 |
| 220Ω Resistors | 14 |
| Breadboard | 1 |
| Jumper wires | ~20 |

---

## 🔌 Wiring

LEDs are connected in order from **A4 (lowest level)** to **pin 13 (peak)**:

```
A4 → A5 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10 → 11 → 12 → 13
```

Each LED:
```
Arduino Pin  →  220Ω Resistor  →  LED Anode (+, long leg)
                                   LED Cathode (-, short leg)  →  GND
```

> ⚠️ **Power tip:** If all 14 LEDs light up simultaneously, the current may exceed what USB alone can safely provide. Consider powering the Arduino from an external 5V source with a shared GND.

---

## 🚀 Quick Start

(unzip all files )

### 1. Flash the Arduino

Open `arduino/audio_led_bar.ino` in **Arduino IDE** and upload it to your Uno.

### 2. Install Python dependencies

```bash
pip install -r python/requirements.txt
```

### 3. Run the GUI

python python/AudioLEDBar.exe
```

1. Select your **microphone** from the dropdown
2. Select the **Arduino COM port** (e.g. `COM5` on Windows, `/dev/ttyUSB0` on Linux)
3. Click **▶ Start** — the equalizer and LEDs come alive!

---

## 📁 Project Structure

```
audio-led-bar/
├── arduino/
│   └── audio_led_bar.ino      # Arduino sketch
├── python/
│   ├── AudioLEDBar.exe        # Main Python application (PyQt5 GUI)
│   └── requirements.txt       # Python dependencies
├── docs/
│   └── wiring_diagram.png     # (add your own wiring photo here)
└── README.md
```

---

## 🎛️ How It Works

```
Microphone  →  Python (FFT analysis)  →  Serial (USB)  →  Arduino  →  LEDs
```

1. **Python** captures audio via `sounddevice`, runs a Fast Fourier Transform (FFT) to split it into 32 frequency bands, and calculates an overall volume level in **dB**.
2. A **noise gate** silences the output when the room is quiet (no flickering LEDs).
3. The volume level is mapped to a number **0–18** and sent to the Arduino over **Serial (9600 baud)**.
4. The **Arduino** turns on that many LEDs in sequence — A4 first (quiet) up to pin 13 (loud peak).

---

## ⚙️ Configuration

All tuneable settings are at the top of `AudioLEDBar.exe`:

| Variable | Default | Description |
|---|---|---|
| `NUM_BARS` | 32 | Equalizer columns shown in GUI |
| `NUM_LEDS` | 14 | Physical LEDs on Arduino |
| `dynamic_range_db` | 38 | dB range from silence to full bar |
| `gate_threshold` | 0.025 | Noise gate — raise if LEDs flicker in silence |
| `SEND_INTERVAL_MS` | 35 | Max Serial send rate (ms) |

---

## 📦 Dependencies

```
PyQt5>=5.15
sounddevice>=0.4
numpy>=1.21
pyserial>=3.5
```

---

## 📄 License

MIT License — free to use, modify, and share.

---

## 🙌 Contributing

Pull requests are welcome! Feel free to open an issue for bugs or feature ideas.
