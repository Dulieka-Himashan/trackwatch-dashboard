# trackwatch-dashboard
Real-time station monitoring dashboard for the Multimodal Intelligent Transportation Safety System — Elephant &amp; Human Train Collision Prevention, Sri Lanka Railways.
# 🐘 TRACKWATCH — Multimodal Intelligent Transportation Safety System
### Elephant & Human Train Collision Prevention · Sri Lanka Railways

---

## 📌 Project Overview

TRACKWATCH is a real-time hazard detection system mounted on a locomotive that detects warm-bodied animals or humans on the railway track using thermal infrared sensing combined with HD video. The system triggers an alarm even in total darkness, rain, or fog — making it effective 24/7 in all weather conditions.

This repository contains the **Station Monitoring Web Dashboard** — the frontend interface used by station operators to monitor live track conditions, view thermal detection data, and track hazard events in real time.

---

## 🚨 The Problem

Sri Lanka has one of the highest rates of elephant-train collisions in the world, particularly at night. Train drivers have almost zero warning time — headlights only illuminate so far, and an elephant's dark skin blends into the environment visually. Existing solutions such as GPS collars, watchtowers, and manual patrols do not scale effectively.

This system detects **body heat**, not visible light — so it works regardless of lighting or weather conditions.

---

## 🏗️ System Architecture

```
AMG8833 Thermal Sensor
        │ (I2C)
    Arduino Uno + Ethernet Shield
        │ (HTTP/JSON over LAN)
    Raspberry Pi 4 (8GB)  ←──── Dahua IP Camera (RTSP / 1080p)
        │
    ┌───┴────────────────────────┐
    │  Python Processing Pipeline │
    │  • Bilinear Interpolation   │
    │  • Jet Colormap             │
    │  • Sensor Fusion Overlay    │
    │  • Threshold Detection >30°C│
    └───┬────────────────────────┘
        │
    12V Relay Module (GPIO)
        │
    LED Indicator / Siren (planned)
        │
    Cloud Event Logging
        │
    ┌───┴──────────────────────────┐
    │  Station Monitoring Dashboard │  ← this repo
    │  Live Feed · Alarm · Log      │
    └──────────────────────────────┘
```

---

## 🧰 Hardware Components

| Component | Role | Spec |
|---|---|---|
| Raspberry Pi 4 (8GB) | Central processing unit | Runs Python, OpenCV, NumPy |
| AMG8833 Grid-EYE | Thermal (LWIR) sensor | 8×8 pixel matrix, 64 heat points |
| Dahua IP Camera | Visual sensor | 1080p HD, RTSP stream |
| Arduino + Ethernet Shield | I2C to HTTP bridge | Serves thermal data as JSON API |
| 5-port Ethernet Switch | Local network hub | Connects all devices on LAN |
| 12V Relay Module | Alarm trigger | Controlled via Pi GPIO |

---

## 💻 Dashboard Features

- 📹 **Live Video Feed** — fused thermal + HD camera stream from the locomotive
- 🌡️ **Thermal Grid Visualisation** — live 8×8 AMG8833 data with jet colormap
- 🚨 **Alarm Status Card** — real-time TRACK CLEAR / HAZARD DETECTED state
- 📋 **Detection Event Log** — timestamped log of all hazard detections
- 📡 **System Status Pills** — Pi Online, Thermal Sensor, Camera Feed, Siren
- 📊 **Live Stats Bar** — last scan time, thermal FPS, video FPS, cloud status

---

## 🔬 Core Algorithms

1. **Bilinear Interpolation** — upscales the 8×8 thermal grid to a smooth 300×300 heat map using OpenCV
2. **Jet Colormap** — maps raw temperature values to colours (blue = cool → red = hot)
3. **Sensor Fusion Overlay** — stamps the thermal heat map onto the HD video frame
4. **Threshold-Based Triggering** — if any pixel exceeds 30°C, alarm is triggered

---

## 🚀 Running the Dashboard

No build step required. Pure HTML/CSS/JS.

```bash
# Just open in browser
open index.html
```

### Connecting to the Live Stream

When the Raspberry Pi stream is active, replace the placeholder YouTube embed in `index.html`:

Find this line:
```html
<iframe id="streamFrame" src="https://www.youtube.com/embed/..." ...></iframe>
```

**For MJPEG stream (HTTP):** replace with:
```html
<img src="http://<PI_IP>:<PORT>/stream" style="width:100%;height:100%;object-fit:cover;" />
```

**For iframe stream:** just replace the `src` value with the Pi's stream URL.

---

## 📡 Communication Protocols

| Connection | Protocol | Data | Frequency |
|---|---|---|---|
| Camera → Pi | RTSP | H.264 Video | 20–30 FPS |
| Arduino → Pi | HTTP GET (JSON) | 8×8 Float Array | ~10 FPS |
| Pi → Dashboard | WebRTC / HTTPS | Processed Video | Real-time |
| AMG8833 → Arduino | I2C | Raw thermal values | Hardware level |

---

## 🔭 Future Scope

- YOLO deep learning model for elephant vs human vs vehicle classification
- Higher resolution thermal sensor (FLIR Lepton 80×60)
- Automatic braking system integration
- Mobile push alerts to station master
- Multi-camera track coverage

---

## 🏫 Academic Context

**University:** NSBM Green University (Affiliated — University of Plymouth, UK)  
**Programme:** BSc (Hons) Software Engineering  
**Year:** 2025 / 2026  

---

*TRACKWATCH — built to protect elephants and save lives on Sri Lanka's railways.*
