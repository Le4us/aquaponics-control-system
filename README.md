# aquaponics-control-system
Arduino-based control system for aquaponics prototype (pH, temperature, water level)
# Aquaponics Control System (Group 16)

## Overview
This repository contains the Arduino-based control system developed for a prototype aquaponics system. The system monitors key water quality parameters and automates responses to maintain stable conditions for both fish and plant growth.

The design supports sustainable water reuse by continuously regulating pH, temperature, and water levels within a closed-loop system.

---

## System Architecture

The control system consists of three main subsystems:

### 1. pH Monitoring and Dosing Control
- Reads analog sensor values and converts them to pH (0–14 scale)
- Compares pH to optimal range (6.6–7.2)
- Calculates required acid or base dosing based on system volume
- Displays status on LCD and alerts user using buzzer

### 2. Temperature Monitoring
- Uses TMP36 sensor to measure water temperature
- Maintains optimal range (22°C – 30°C)
- Provides real-time feedback via LCD display
- Activates buzzer if temperature is outside safe limits

### 3. Water Level Control
- Uses ultrasonic sensor to measure tank water level
- Detects low water conditions using distance threshold
- Automatically opens a servo-controlled valve to refill tank
- Closes valve after a fixed duration to prevent overfilling

---

## Control Logic

The system follows a consistent control process:

1. Sensor data is collected (pH, temperature, water level)
2. Signals are converted into physical values
3. Values are compared to predefined safe operating ranges
4. If outside range:
   - Alerts are triggered (buzzer + LCD)
   - Corrective actions are calculated or executed
5. System continuously updates in real-time

---

## Hardware Components

- Arduino Uno
- LCD Display (I2C)
- TMP36 Temperature Sensor
- Analog pH Sensor (prototype / simulated)
- Ultrasonic Distance Sensor
- Servo Motor (valve control)
- Piezo Buzzer

---

## Files

- `aquaponics_code.md` → Full Arduino code for all subsystems
  - pH control
  - Temperature monitoring
  - Water level control

---

## Design Considerations

- Low-cost and accessible components for prototyping
- Modular subsystem design for easy expansion
- Real-time monitoring and feedback
- Minimal energy consumption
- Simple user interface (LCD + alerts)

---

## Limitations

- pH sensor is a proof-of-concept (simulated/approximate mapping)
- No automated chemical dosing hardware implemented
- Environmental factors (light, humidity) not controlled in this prototype
- Manual calibration may be required for accurate readings

---

## Future Improvements

- Integration of real pH probe with calibration
- Automated dosing pumps for precise pH correction
- Data logging and remote monitoring (IoT integration)
- Closed-loop feedback control instead of timed actions
- Improved filtration and nutrient balancing algorithms

---

## Author

Group 16 – Aquaponics Prototype Project  
Western University Engineering  

---
