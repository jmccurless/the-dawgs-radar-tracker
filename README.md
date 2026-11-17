**Course:** IT 254 – Hardware and Software Concepts (Fall 2026)  
**Group:** Group 3 – The Dawgs  
**Author:** Jack Curless (Individual Project Repository & Documentation)  
**Team Members:** Jack Curless, Owen M, Aaron B, Uriah Blair, Jon H  

---

## 1. Project Overview
This project implements an offline, real-time spatial tracking and classification pipeline on a **Raspberry Pi 5** using a **24GHz mmWave Radar Sensor (HLK-LD2450)**. The system tracks multi-target trajectories via UART telemetry and uses a lightweight neural network to:
1. Differentiate between human occupants by height, walking, running, falling, and crawling.
2. Possible applications: fire and rescue, police situations, security operations.

---

## 2. Hardware Architecture & Bill of Materials
* **Compute Node:** Raspberry Pi 5

* **Radar Sensor:** [Hi-Link HLK-LD2450 24GHz mmWave Human Presence & Trajectory Sensor](https://www.hlktech.net/index.php?id=988) ([Amazon Listing](https://a.co/d/07EvIT1W))
* **Microcontroller / Peripherals:** Arduino & External Display
* **Custom Enclosure / Mount:** 3D-printed sensor stand ([Tinkercad 3D Model](https://www.tinkercad.com/things/hxSJa5kXIYv-the-dawgs-stand/edit))
  * **Dimensions:** 45.0 mm tall × 20.0 mm wide
  * **Slot Tolerance:** Minimum 2.0 mm thickness required for sensor fitment

### HLK-LD2450 to Raspberry Pi 5 GPIO Pinout
The radar communicates with the Raspberry Pi 5 over serial UART (`115200` baud default for LD2450):

| HLK-LD2450 Pin | Function | Raspberry Pi 5 GPIO Pin | Physical Pin Number |
| :--- | :--- | :--- | :--- |
| **5V** | Power Supply (5V) | 5V Power | **Pin 2 or 4** |
| **GND** | Ground | Ground (GND) | **Pin 6, 9, 14, or 20** |
| **TX** | Radar Transmitter | **GPIO 15 (RXD)** | **Pin 10** |
| **RX** | Radar Receiver | **GPIO 14 (TXD)** | **Pin 8** |

---

## 3. System Setup & Environment Configuration

### I2C & Serial Interface Verification
```bash
sudo apt update
sudo apt install i2c-tools
i2cdetect -y 1
```

  ### AI Runtime, Model Deployment & Troubleshooting

**Pre-First Meeting Preparations**
* Discussed initial project ideas and finalized our hardware requirements.
* Sourced and ordered the HLK-LD2450 24GHz mmWave sensor on amazon.
* Uriah Researched the required GPIO pinouts and cabling to properly interface the sensor with the Raspberry Pi.

**First Group Meeting (10/24)**
* Verified the physical connectivity between the sensor and the Pi.
* Utilized Gemini to generate an initial Python script to test and establish the connection.
* Created a follow up script to parse the raw hexadecimal output from the sensor into human-readable data.
* Iterated through four different versions of the parsing script until the data output was concise, clean, and easy to read.

**Second Group Meeting (11/3)**
* Fully wired the sensor to the Raspberry Pi 5.
* Experimented with local AI runtimes: initially installed Ollama, but uninstalled it due to high overhead and switched to Homebrew and llama.cpp.
* Switched from general Liquid Foundation Models (LFMs) to a PyTorch-based Closed-Form Continuous-Time (CfC) neural network. This shift significantly reduced RAM usage and inference latency on the Pi 5.
* Ran our first AI inference script and tweaked the code so the object's tracked location would be clearly presentable for our demo for humans to read
* Ran one version of the training data script. Found a problem and actually collected 0 data. Turned out the sensor connections to the PI had been messed up
* Worked with Aaron to capture the initial baseline training data, allowing the CfC model to accurately distinguish between different states like **no movement** and **1 person**.
