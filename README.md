**Course:** IT 254 – Hardware and Software Concepts (Fall 2026)  
**Group:** Group 3 – The Dawgs  
**Author:** Jack Curless (Individual Project Repository & Documentation)  
**Team Members:** Jack Curless, Owen M, Aaron B, Uriah Blair, Jon H  

---

## 1. Project Overview
This project implements an offline, real-time spatial tracking and classification pipeline on a **Raspberry Pi 5** using a **24GHz mmWave Radar Sensor (HLK-LD2450)**. The system tracks multi-target trajectories via UART telemetry and uses a lightweight neural network to:
1. Differentiate between human occupants and household pets.
2. Detect sudden human falls and trigger an appropriate local alert/display response.

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
### AI Runtime & Model Deployment Evolution
* Installed Ollama for local model execution, then deleted it and moved to Homebrew and llama.cpp
* Transitioned to from general liquid Foundation Models (LFMs) to a Closed-Form Continuous-Time (CfC) neural network implemeted in PyTorch
(We decided to move to CfC model becasue it reduced RAM usage and inference latency on the Pi5 because it cut down on unnecassary LFM uses)
* Worked with Aaron to run the first base training set so the Cfc could distinguish from (no movment, 1 person, and 2 people)
