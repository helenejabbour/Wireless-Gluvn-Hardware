# 🎼 GLUVN Hardware
### Custom Hardware Platform for the M5-GLUVN Wireless Musical Glove

> A custom wearable electronics platform that transforms hand movements into expressive musical performance.

---

## Overview

**GLUVN Hardware** contains all of the hardware design files for the **M5-GLUVN** project, a wireless wearable musical instrument based on the **M5Stick C Plus 1.1**.

The hardware was designed from the ground up to overcome the limited analog inputs of the ESP32 while maintaining low latency, compact size, and reliable sensor acquisition.

Each glove measures:

- Finger bending using Flex Sensors
- Finger pressure using Force Sensitive Resistors (FSRs)
- Hand orientation using the built-in IMU inside the M5Stick

The collected data is transmitted over **Bluetooth Low Energy (BLE)** to the main GLUVN software, where it is converted into MIDI events for real-time musical performance.

---

# Features

- Custom PCB designed in KiCad
- Compact wearable design
- M5Stick C Plus 1.1 support
- ESP32 BLE connectivity
- CD4067 analog multiplexer
- Reads 10 analog sensors using a single ADC pin
- Separate analog conditioning circuits for Flex and FSR sensors
- Built-in IMU support
- USB-C programming and charging
- Designed specifically for real-time musical interaction

---

# Hardware Architecture

```
               FLEX SENSORS (5)
                     │
                     │
               FSR SENSORS (5)
                     │
                     ▼
          Analog Signal Conditioning
        (Separate RC filters per sensor)
                     │
                     ▼
              CD4067 Multiplexer
                     │
             Single Analog Output
                     │
                     ▼
            M5Stick C Plus 1.1
          (ESP32 + BLE + IMU + ADC)
                     │
             Bluetooth Low Energy
                     │
                     ▼
             GLUVN Software (PC)
                     │
                     ▼
                   MIDI
```

---

# Why a Multiplexer?

The ESP32 inside the M5Stick provides only a few ADC inputs, while each glove requires reading:

- 5 Flex Sensors
- 5 Pressure Sensors

Instead of using a larger microcontroller, a **CD4067 16-channel analog multiplexer** allows all sensors to share a single ADC input while remaining fast enough for musical applications.

Benefits include:

- Lower cost
- Smaller PCB
- Fewer GPIO requirements
- Simpler routing
- Expandable architecture

---

# Sensor Configuration

Each glove contains:

| Sensor | Quantity |
|----------|---------:|
| Flex Sensors | 5 |
| Force Sensitive Resistors | 5 |
| Total Analog Inputs | 10 |

The M5Stick additionally provides:

- 3-axis Accelerometer
- 3-axis Gyroscope

These can be used for:

- Volume
- Pitch bend
- Expression
- Modulation
- Gesture recognition

---

# Analog Front-End

Different sensors require different filtering characteristics.

## Flex Sensors

- Pull-down resistor: **47 kΩ**
- Capacitor: **100 nF**
- Time constant: **4.7 ms**

The larger filter smooths finger bending while reducing noise.

---

## FSR Sensors

- Pull-down resistor: **10 kΩ**
- Capacitor: **10 nF**
- Time constant: **100 μs**

This provides much faster response for expressive finger pressure.

---

# Multiplexer Configuration

```
CH0   → FSR Thumb
CH1   → FSR Index
CH2   → FSR Middle
CH3   → FSR Ring
CH4   → FSR Pinky

CH5   → Flex Thumb
CH6   → Flex Index
CH7   → Flex Middle
CH8   → Flex Ring
CH9   → Flex Pinky

CH10–15 Reserved
```

Control pins:

```
S0
S1
S2
S3
```

Output:

```
MUX SIG → ESP32 ADC
```

---

# PCB Features

- Compact two-layer PCB
- Dedicated analog ground routing
- Sensor connectors
- M5Stick mounting headers
- Decoupling capacitors
- Separate RC networks
- USB accessibility
- Wearable footprint

---

# Repository Structure

```
GLUVN-Hardware/

├── pcb/
│   ├── KiCad/
│   ├── Gerbers/
│   ├── Assembly/
│   └── Manufacturing/
│
├── schematics/
│   ├── PDF/
│   └── Source/
│
├── bom/
│   ├── BOM.csv
│   └── BOM.xlsx
│
├── docs/
│   ├── Assembly_Guide.md
│   ├── Wiring.md
│   ├── Design_Notes.md
│   └── Testing.md
│
├── enclosure/
│   ├── STEP/
│   ├── STL/
│   └── CAD/
│
├── images/
│   ├── PCB_Render_Front.png
│   ├── PCB_Render_Back.png
│   ├── Assembled_Board.jpg
│   ├── Schematic.png
│   └── System_Diagram.png
│
└── README.md
```

---

# Manufacturing

This repository includes everything required for fabrication.

Included files:

- KiCad project
- Schematics
- Gerber files
- Drill files
- Bill of Materials
- Pick-and-Place files
- 3D models
- Assembly drawings

The board can be manufactured by any PCB fabrication service such as:

- JLCPCB
- PCBWay
- OSH Park
- Seeed Fusion

---

# Assembly

## 1. Manufacture the PCB

Use the provided Gerber files or generate your own from KiCad.

---

## 2. Solder Passive Components

Install:

- Resistors
- Capacitors
- Decoupling capacitors

---

## 3. Install the CD4067

Carefully align pin 1 before soldering.

---

## 4. Install Headers

Install:

- M5Stick headers
- Sensor connectors
- Programming headers (if applicable)

---

## 5. Mount the M5Stick

Connect the M5Stick securely to the PCB.

---

## 6. Connect Sensors

Attach:

- Five Flex Sensors
- Five FSR Sensors

Verify correct channel mapping.

---

## 7. Upload Firmware

Firmware is **not included** in this repository.

Please use the firmware from the main GLUVN repository.

---

## 8. Calibrate

Run the calibration utility from the software repository before first use.

---

# Design Goals

The hardware was designed with the following priorities:

- Low latency
- Small size
- Low cost
- Easy manufacturing
- Easy assembly
- Reliable analog measurements
- Expandability
- Wearability

---

# Known Limitations

- Designed specifically for the M5Stick C Plus 1.1
- Requires calibration before use
- Uses one analog multiplexer per glove
- Not waterproof
- Sensor performance depends on glove construction

---

# Future Improvements

Possible future revisions include:

- ESP32-S3 support
- Higher resolution ADC
- Integrated battery charging
- Capacitive touch sensing
- Hall-effect finger sensors
- Flexible PCB version
- Wireless charging
- Dedicated enclosure
- Four-layer PCB
- Integrated haptic feedback

---

# Companion Repositories

This repository contains **hardware only**.

The complete project consists of multiple repositories:

| Repository | Description |
|------------|-------------|
| **GLUVN-Hardware** | PCB, schematics, manufacturing files |
| **GLUVN Firmware** | ESP32 firmware for the M5Stick |
| **GLUVN Software** | Python application, BLE communication, calibration tools, MIDI engine, visualization |

---

# Images

The following images are recommended for this repository.

- Complete assembled system
- Front PCB render
- Back PCB render
- Schematic overview
- System architecture
- Wiring diagram
- Finished wearable glove

---

# Contributing

Contributions are welcome.

Examples include:

- PCB improvements
- Hardware revisions
- Assembly documentation
- Better sensor integration
- Wearable design improvements
- Cost reduction
- Testing and validation

Please open an Issue or Pull Request to discuss proposed changes.

---

# License

This hardware design is released under the **MIT License** unless otherwise specified.

---

# Acknowledgments

This project is part of the **M5-GLUVN** initiative, which explores expressive wearable musical interfaces by combining custom electronics, embedded systems, Bluetooth communication, and real-time MIDI generation.

If you build your own version, feel free to share it—we'd love to see what you create!
