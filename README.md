# Alona IoT — Firmware

Firmware for **Alona IoT** ESP32-based sensor nodes.

Each node runs a small embedded application that:

- reads data from connected sensors
- publishes telemetry and events via MQTT
- sends periodic heartbeats
- supports offline detection via MQTT last-will messages

The firmware is designed for low power usage and unreliable network conditions, typical of off-grid environments.

## Responsibilities

- Sensor data acquisition
- MQTT publishing (telemetry, events, heartbeats)
- Device identification and provisioning
- Network reconnect and retry logic

## Supported Hardware

- ESP32-based boards
- Various environmental, energy, water, and security sensors

## Related Repositories

- `core` — backend ingestion and storage
- `protocol` — topic structure and payload formats
