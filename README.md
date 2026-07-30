# WALL-E Replica Robot 🤖

A school project replica of **WALL-E**, built to traverse hard terrain (like stairs) using a unique **six-wheel, tri-star rocker design** (three wheels per side). The robot features a movable head, dual-Arduino control architecture, RC control, obstacle sensing, and live wireless telemetry to an onboard display.

![WALL-E Final Build](images/wallie-final-build.jpeg)

---

## ✨ Features

- **Terrain-climbing wheel design** — 3 wheels per side in a rocker-style arrangement, allowing the robot to climb stairs and uneven surfaces.
- **Dual Arduino Mega architecture** — a Master and a Slave Arduino, each handling a dedicated subsystem.
- **Obstacle/terrain sensing** — TF Mini LiDAR + Bosch 9-DOF motion shield (accelerometer, gyroscope, magnetometer) on the Master Arduino.
- **Movable head** — driven by a 60kg-torque servo motor, controlled over a separate Bluetooth receiver channel.
- **RC-controlled drive system** — FlySky RC receiver + Cytron MDDS30 dual-channel motor driver powering two Johnson 300 RPM motors.
- **Live wireless data display** — HC-05 Bluetooth module streams sensor data to a 16x2 I2C LCD on the Slave Arduino.

---

## 🛠️ Hardware Components

| Component | Quantity | Purpose |
|---|---|---|
| Arduino Mega 2560 | 2 (Master + Slave) | Main controllers |
| Bosch 9-Axis (9-DOF) Motion Shield | 1 | Accelerometer / gyroscope / magnetometer data for terrain orientation |
| TF Mini LiDAR | 1 | Distance/obstacle sensing (UART) |
| Cytron MDD30 (SmartDriveDuo-30) | 1 | Dual-channel motor driver |
| Johnson 300 RPM DC Motors | 2 | Drive motors |
| 60kg Servo Motor | 1 | Head movement |
| FlySky RC Receiver | 1 | Manual drive/head control |
| HC-05 Bluetooth Module | 1 | Wireless telemetry link |
| 16x2 I2C LCD Display | 1 | Displays data received over Bluetooth |
| Buck Converter (12V → 8V) | 1 | Regulated power for the Arduinos |
| 12V Power Supply / Battery | 1–2 | Main power source |

---

## 🧠 System Architecture

The robot runs on **two Arduino Mega boards**, splitting responsibilities to keep each controller's workload manageable:

### Master Arduino
- Reads the **TF Mini LiDAR** over UART (RX0/TX1 on pins 18 & 19) for obstacle/distance sensing.
- Reads the **Bosch 9-Axis Motion Shield** over I2C (SDA → A4, SCL → A5) for orientation data (X/Y acceleration and other motion parameters).
- Handles core sensor fusion and terrain-awareness logic.

### Slave Arduino
- Connected to the **HC-05 Bluetooth module**, receiving data forwarded from the Master (or a paired controller app).
- Drives a **16x2 I2C LCD** to display the incoming telemetry/status data in real time.

### Drive & Actuation
- The **Cytron MDDS30** motor driver controls both **Johnson 300 RPM motors** for the drive wheels, powered directly from the 12V supply.
- A **FlySky RC receiver** provides manual driving control.
- The **60kg servo** (head movement) is driven from a separate channel on the Bluetooth receiver, allowing wireless control of the head independently of the drivetrain.

### Power System
- A single **12V power supply/battery** feeds a **buck converter**, stepping the voltage down to ~8V to safely power the Arduino boards.
- The drive motors and motor driver are powered directly from the 12V line for full torque.

---

## 🔌 Circuit Diagrams

### 1. Master Arduino — Motion Shield + TF Mini LiDAR
Connects the 9-Axis Motion Shield (I2C: A4/A5) and the TF Mini LiDAR (UART: TX→RX0, RX→TX1, pins 18 & 19) to the Master Arduino Mega.

![Master Arduino Wiring](images/master-arduino-wiring.png)

### 2. Power Distribution
12V supply → buck converter (steps down to 8V) → powers both Arduino Mega boards.

![Power Supply Wiring](images/power-supply-wiring.png)

### 3. Motor Driver, RC Receiver & Head Servo
Cytron MDDS30 driving both Johnson 300 RPM motors from the 12V battery, with the FlySky RC receiver and the head servo wired into the driver's signal side.

![Motor Driver Wiring](images/motor-driver-wiring.png)

### 4. Slave Arduino — Bluetooth + LCD
HC-05 Bluetooth module and 16x2 I2C LCD wired to the Slave Arduino Mega for wireless data display.

![Slave Arduino Wiring](images/slave-arduino-wiring.png)

---

## 📁 Repository Structure

```
├── README.md
└── images/
    ├── master-arduino-wiring.png
    ├── power-supply-wiring.png
    ├── motor-driver-wiring.png
    ├── slave-arduino-wiring.png
    └── wallie-final-build.jpeg
```

*(Add your `.ino` sketches for the Master and Slave Arduinos to the repo and link them here once uploaded.)*

---

## 🚀 About

This project was built as a school assignment to replicate WALL-E's look and mobility — with a working stair-capable wheelbase, a movable head, live obstacle sensing, and wireless status reporting.
