# PetTrackingCollar

IoT pet-tracking collar prototype built with ESP32. The device publishes telemetry (temperature + GPS) to MQTT and listens for remote alert commands (buzzer + LED) from a dashboard.

## Overview
This project demonstrates a lightweight pet monitoring flow:
- ESP32 connects to Wi-Fi and MQTT broker
- Publishes temperature and location periodically
- Receives `ringBell` command from dashboard
- Activates buzzer/LED to help locate pets quickly

## Tech Stack
- ESP32 + Arduino C/C++
- MQTT (`PubSubClient`)
- DHT22 (`DHTesp`)
- Node-RED dashboard flow (`nodered.json`)
- HiveMQ public broker
- Wokwi simulation assets (`diagram.json`, `wokwi-project.txt`)

## Project Structure
- `sketch.ino`: Main firmware logic
- `libraries.txt`: Required Arduino/Wokwi libraries
- `nodered.json`: Node-RED flow
- `diagram.json`: Wokwi wiring diagram
- `MeoCare.stl`: Collar enclosure model

## MQTT Topics
- `21127577/temperature`: Device publishes temperature (Celsius)
- `21127577/gps`: Device publishes GPS JSON, e.g. `{"lat":10.749960,"lon":106.696205}`
- `21127577/ringBell`: Dashboard publishes `true/false` to control buzzer + LED

## Hardware Pins (ESP32)
- `D15`: DHT22 data pin
- `D13`: LED pin
- `D12`: Buzzer pin

## How It Works
1. ESP32 connects to Wi-Fi.
2. ESP32 connects to MQTT broker and subscribes to `ringBell` topic.
3. Every ~5 seconds, firmware reads DHT22 temperature and publishes MQTT telemetry.
4. GPS coordinates are currently simulated in firmware.
5. When `ringBell=true`, buzzer and LED turn on; `false` turns them off.

## Quick Start
## 1) Firmware
- Open `sketch.ino` in Arduino IDE (or Wokwi).
- Install dependencies listed in `libraries.txt`.
- Update Wi-Fi / broker config if needed:
  - `ssid`
  - `password`
  - `mqttServer`
  - `mqttPort`
- Flash to ESP32 (or run simulation).

## 2) Dashboard (Node-RED)
- Import `nodered.json` into Node-RED.
- Configure MQTT nodes to same broker/topics.
- Use dashboard button/switch to publish `true/false` to `21127577/ringBell`.

## Notes
- The current GPS values are static placeholders in `sketch.ino`.
- For real-world tracking, replace simulated coordinates with a real GPS module input (e.g., NEO-6M).
- Public brokers are useful for demos; use authenticated/private brokers in production.
