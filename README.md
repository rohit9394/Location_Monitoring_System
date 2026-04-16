# 🛰️ Real-Time Soldier GPS Tracking with ML-Based Error Prediction

## 📌 Overview

This project implements a **real-time soldier tracking system** that provides:

* Live GPS position tracking
* Telemetry monitoring
* Machine Learning-based GPS error prediction

The system integrates **ESP32 + NavIC GPS + LoRa + Raspberry Pi + Flask + ML model** to deliver a complete end-to-end solution.

As described in the project report , the system achieves a **mean absolute error of 0.469 meters** in predicting GPS inaccuracies.

---

## 🧠 Key Features

* 📍 Real-time GPS tracking using NavIC
* 📡 Long-range communication via LoRa (SX1262)
* 🧮 Kalman Filter for noise reduction
* 🤖 Random Forest model for GPS error prediction
* 🌐 Live web dashboard using Flask + Leaflet
* 🎯 Error visualization using color-coded uncertainty circle
* 📊 Telemetry display (speed, altitude, satellites, HDOP)

---

## 🏗️ System Architecture

```
ESP32 (Soldier Unit)
        ↓ (LoRa)
Raspberry Pi (Receiver + ML + Flask API)
        ↓ (HTTP)
Web Dashboard (Browser)
```

---

## 🔧 Hardware Components

* ESP32 (Soldier unit)
* NavIC GPS Module
* SX1262 LoRa Module
* Raspberry Pi (Base station + server)

---

## 💻 Software Components

### 1. ESP32 Firmware

* Reads GPS data using TinyGPS++
* Sends structured packets via LoRa

### 2. Raspberry Pi (Base Station)

* Receives LoRa packets
* Applies Kalman filtering
* Runs ML model for error prediction
* Serves data via Flask API

👉 Implemented in: `rpi.py` 

---

### 3. Dashboard Server

* Polls Raspberry Pi API every second
* Displays map, telemetry, and error circle

👉 Implemented in: `app.py` 
👉 Frontend: `templates/index.html`

---

### 4. Data Logging

* Logs raw GPS data via serial for training

👉 File: `csv_logging.py` 

---

### 5. Machine Learning Model

* Model: Random Forest Regressor
* Features include:

  * Speed, altitude, course
  * Satellites, HDOP
  * Engineered features (speed × satellites, ratios)

📊 Performance:

* R² Score: 0.968
* MAE: 0.469 m
* RMSE: 0.796 m

---

## 📡 API Endpoints

### `/data`

Returns real-time telemetry and predicted error:

```json
{
  "lat": 26.12,
  "lon": 73.12,
  "speed": 4.2,
  "altitude": 345,
  "sats": 12,
  "hdop": 1.2,
  "prediction": 0.45
}
```

---

## 🚀 How to Run

### 1️⃣ ESP32 Setup

* Upload `soldier_monitoring_system.ino`
* Connect GPS + LoRa module

---

### 2️⃣ Raspberry Pi (Receiver + ML)

```bash
python rpi.py
```

---

### 3️⃣ Dashboard Server

```bash
python app.py
```

---

### 4️⃣ Open Dashboard

```
http://<raspberry-pi-ip>:5000
```

---

## 📁 Project Structure

```
├── app.py                  # Flask dashboard server
├── rpi.py                  # Base station + ML prediction
├── templates/
│   └── index.html          # Frontend dashboard
├── csv_logging.py          # Data logging script
├── model.ipynb             # ML training notebook
├── gps_error_model.joblib
├── model_features.joblib
└── soldier_monitoring_system.ino
```

---

## 📊 Dashboard Features

* 🗺️ Live map (Leaflet)
* 📍 Real-time marker tracking
* 🔵 Path tracking (polyline)
* 🟢🟡🔴 Error circle:

  * Green: < 2m
  * Yellow: 2–5m
  * Red: > 5m
* 📈 Telemetry panel
* 📏 Distance calculation

---

## 🔬 Machine Learning Pipeline

1. Data collection (17,597 GPS points)
2. Map matching using OSRM
3. Kalman filtering
4. Feature engineering
5. Random Forest training
6. Real-time inference on Raspberry Pi

---

## ⚡ Performance

* Prediction latency: ~2 seconds
* High accuracy even in noisy environments
* Works in both urban and open-field conditions

---

## 🧑‍💻 Authors

* Rohit Sharma
* Namruth Tej
* Chevula Ranjith

---

## 📌 Future Improvements

* Edge AI deployment on ESP32
* Multi-soldier tracking
* Mobile app integration
* Alert system for high error zones

---

## 📜 License

This project is for academic and research purposes.

---

## ⭐ Acknowledgment

Inspired by real-world defense and navigation challenges, combining **IoT + Embedded Systems + Machine Learning** into one unified system.
