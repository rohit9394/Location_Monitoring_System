# 🛰️ Real-Time Soldier GPS Tracking with ML-Based Error Prediction

## 📌 Overview

This project implements a **real-time soldier tracking system** that provides:

* Live GPS tracking
* Telemetry monitoring
* Machine Learning-based GPS error prediction

The system integrates **ESP32 + NavIC GPS + LoRa + Raspberry Pi + Flask + ML model** into a complete pipeline.

According to the updated report , the system achieves:

* **MAE: 0.365 meters**
* **R² Score: 0.982**

---

## 🧠 Key Features

* 📍 Real-time GPS tracking (NavIC)
* 📡 LoRa-based long-range communication
* 🧮 Kalman filtering with optimized parameters
* 🤖 Random Forest ML model for error prediction
* 🌐 Live dashboard using Flask + Leaflet
* 🎯 Error visualization (color-coded circle)
* 📊 Real-time telemetry panel

---

## 🏗️ System Architecture

```id="arch123"
ESP32 (Soldier Unit)
        ↓ (LoRa)
Raspberry Pi (Receiver + Kalman + ML + Flask API)
        ↓ (HTTP)
Web Dashboard (Browser)
```

---

## 🔧 Hardware Components

* ESP32 (Soldier unit)
* NavIC GPS module
* SX1262 LoRa module
* Raspberry Pi (Base station + server)

---

## 💻 Software Components

### 1. Soldier Unit (ESP32)

* Reads GPS using TinyGPS++
* Sends structured LoRa packets every 2 seconds

---

### 2. Base Station (Raspberry Pi)

* Receives LoRa data
* Applies **5 independent Kalman filters**
* Runs ML model for GPS error prediction
* Serves JSON API (`/data`)

👉 File: `rpi.py`

---

### 3. Dashboard Server

* Polls data every second
* Displays map, telemetry, and error circle

👉 Files:

* `app.py`
* `templates/index.html`

---

### 4. Data Logging

* Logs GPS data for training

👉 File: `csv_logging.py`

---

### 5. Machine Learning Model

* Model: Random Forest Regressor
* Trained on **17,597 GPS points**

### 📊 Performance

* R² Score: **0.9823**
* MAE: **0.365 m**
* RMSE: **0.646 m**

---

## 🧪 Kalman Filter Optimization

The Kalman filter parameters were tuned using grid search:

* **Q (process noise)** = 1e-5
* **R (measurement noise)** = 0.5

This combination gave the best trade-off between:

* Smoothness
* Responsiveness
  (as discussed in the report , page 8–9)

---

## 📡 API Endpoint

### `/data`

Returns real-time telemetry + prediction:

```json id="api123"
{
  "lat": 26.12,
  "lon": 73.12,
  "speed": 4.2,
  "altitude": 345,
  "sats": 12,
  "hdop": 1.2,
  "prediction": 0.36
}
```

---

## 🚀 How to Run

### 1️⃣ ESP32 Setup

* Upload `soldier_monitoring_system.ino`
* Connect GPS + LoRa

---

### 2️⃣ Raspberry Pi (Receiver + ML)

```bash id="run1"
python rpi.py
```

---

### 3️⃣ Dashboard Server

```bash id="run2"
python app.py
```

---

### 4️⃣ Open Dashboard

```id="run3"
http://<raspberry-pi-ip>:5000
```

---

## 📁 Project Structure

```id="struct123"
├── app.py
├── rpi.py
├── templates/
│   └── index.html
├── csv_logging.py
├── model.ipynb
├── gps_error_model.joblib
├── model_features.joblib
└── soldier_monitoring_system.ino
```

---

## 📊 Dashboard Features

* 🗺️ Live map (Leaflet)
* 📍 Real-time tracking
* 🔵 Path visualization
* 🟢🟡🔴 Error circle:

  * Green: < 2m
  * Yellow: 2–5m
  * Red: > 5m
* 📈 Telemetry panel
* 📏 Distance calculation
* 🔌 Connection status indicator

---

## 🔬 ML Pipeline

1. Data collection (17,597 points)
2. OSRM map matching for ground truth
3. Kalman filtering
4. Feature engineering
5. Random Forest training
6. Real-time prediction

---

## ⚡ Communication Flow

```
ESP32 → LoRa → Raspberry Pi → Flask API → Browser
```

* End-to-end latency ≈ **2 seconds**
* Real-time updates every second

---

## ⚠️ Limitations

(From updated report , page 14–15)

* 📡 **Limited LoRa range**

  * ~200m (LOS), ~150m (NLOS)
* 🏢 **No indoor operation**

  * GPS signal loss inside buildings
* ❄️ **Sensor freeze issue**

  * Stale data when GPS signal is lost

---

## 🔮 Future Improvements

* Indoor positioning (IMU / WiFi-based)
* Multi-soldier tracking system
* Signal-loss detection system
* External LoRa antennas for extended range

---

## ⭐ Final Note

This project combines **Embedded Systems + IoT + Machine Learning + Web Development** into a unified real-time system, demonstrating practical deployment of AI on edge-connected devices.
