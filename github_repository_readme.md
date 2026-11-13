# Virtual Smart Energy Analytics System (IoT & Embedded Simulation)

An end-to-end IoT and embedded simulation framework designed to calculate, monitor, and analyze smart grid power consumption metrics in real-time without requiring physical hardware sensors. Built using an **ESP32 microcontroller** simulated in **Wokwi**, integrated via the **MQTT protocol** and **ThingSpeak Cloud**, and visualized through a real-time responsive web dashboard.

---

## 🚀 Project Overview

Energy optimization is critical for modern smart homes and industrial infrastructures. This project models a complete AC power monitoring ecosystem entirely within a virtualized simulation environment. It simulates grid conditions, performs mathematical computations locally on the edge device, streams telemetry packets to cloud and MQTT brokers, and triggers automated overcurrent warnings.

---

## 🛠️ System Architecture & Phased Methodology

The project is structured across three core development phases:

### Phase 1: Data Simulation & Local Edge Processing
* **AC Grid Voltage Simulation:** Generates virtual RMS voltage ($220-240\text{V}$) with Gaussian noise.
* **Dynamic Load Toggling:** Utilizes an ESP32 push-button pin (GPIO 4 with internal pull-up) to switch between normal residential loads (\~$2.5\text{A}$) and heavy industrial loads (\~$8.5\text{A}$).
* **Power & Energy Computations:** Calculates active power in real-time ($P = V \times I \times \text{PF}$, where $\text{PF} = 0.92$) and numerically integrates cumulative energy consumption in kilowatt-hours ($\text{kWh}$).
* **Local Display:** Outputs live status instantly to an **I2C 1602 LCD screen** (SDA: GPIO 21, SCL: GPIO 22).

### Phase 2: IoT Cloud Integration & Communication Protocols
* **Wi-Fi Connectivity:** Connects the simulated ESP32 to virtual Wi-Fi (`Wokwi-GUEST`).
* **MQTT & Cloud Publishing:** Publishes structured JSON telemetry payloads (`voltage`, `current`, `power`, `energy_kwh`, `alert`) to public brokers (`broker.hivemq.com`) and syncs historical records to **ThingSpeak IoT Cloud Channels** via HTTP REST APIs.

### Phase 3: Analytics & Web Dashboard Visualization
* **Automated Overcurrent Alerting:** Continuously evaluates RMS current against safety thresholds ($7.5\text{A}$). If exceeded, it triggers immediate warning flags and publishes alerts to `smart-energy/alerts`.
* **Real-Time Web Dashboard:** A responsive HTML/JavaScript dashboard (`dashboard.html`) connects via WebSockets (`wss://broker.hivemq.com:8884/mqtt`) to visualize live grid metrics and flash warning banners during overloads.

---

## 🧰 Tech Stack & Tools

* **Microcontroller Simulation:** Wokwi (ESP32 DevKit V1)
* **Firmware IDE:** Arduino IDE (C++ / FreeRTOS-compatible execution loops)
* **Libraries:** `WiFi.h`, `PubSubClient.h`, `Wire.h`, `LiquidCrystal_I2C.h`, `HTTPClient.h`
* **Communication Protocols:** MQTT, HTTP REST, TCP/IP, WebSockets
* **Cloud & Visualization:** ThingSpeak IoT Platform, HiveMQ Public Broker, Custom HTML/JS UI

---

## 📂 Repository Structure

```tree
├── sketch.ino            # ESP32 firmware (Simulation, Math, MQTT & HTTP client)
├── libraries.txt         # Wokwi dependency list (PubSubClient, LiquidCrystal_I2C)
├── diagram.json          # Wokwi hardware schematic configuration
└── dashboard.html        # Web-based MQTT analytics and alert visualization UI
```

---

## ⚡ Getting Started & Setup Guide

### 1. Hardware Simulation Setup (Wokwi)
1. Open [Wokwi](https://wokwi.com) and create a new **ESP32 DevKit V1** project.
2. Add an **I2C LCD 1602** display (`SDA` $\rightarrow$ GPIO 21, `SCL` $\rightarrow$ GPIO 22).
3. Add a **Push Button** connected to **GPIO 4** (configured with internal pull-up) to simulate heavy load state switching.
4. Paste the firmware code into `sketch.ino` and add required libraries to `libraries.txt`.

### 2. Running the Web Dashboard
1. Save the `dashboard.html` file locally on your machine.
2. Open `dashboard.html` in any modern web browser.
3. Start the simulation in Wokwi. The dashboard will automatically connect via WebSockets and display live telemetry metrics.
4. Click the push button in Wokwi to test the automated overcurrent alert banner ($>7.5\text{A}$).

---

## 📜 License

This project is open-source and available under the [MIT License](LICENSE).