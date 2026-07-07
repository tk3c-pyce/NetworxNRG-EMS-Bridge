# NetworxNRG EMS Bridge

## Overview

NetworxNRG EMS Bridge is a modular Python platform that connects the NetworxNRG Energy Management System (EMS) to field devices such as photovoltaic (PV) inverters, battery energy storage systems (BESS), power conversion systems (PCS), smart meters and data loggers.

The platform communicates with the central EMS via MQTT and with field equipment using Modbus TCP, Modbus RTU and vendor-specific communication protocols.

The project is designed to support multiple hardware vendors while maintaining a common MQTT interface towards the EMS.

---

# Project Philosophy

The project consists of two main components:

## 1. MQTT Bridge

The MQTT Bridge is intended primarily for photovoltaic (PV) plants without battery storage.

Its responsibilities are:

* Read telemetry from PV inverters.
* Read telemetry from smart meters.
* Publish telemetry, status and summary data to the EMS.
* Receive commands from the EMS.
* Apply active power limitation (active_power_percent) to PV inverters.
* Report alarms, warnings and communication status.

Typical supported devices include:

* Huawei SmartLogger
* Azzurro / Sofar
* Growatt
* KACO
* Solplanet
* KSTAR
* Other Modbus-compatible PV equipment

The Bridge itself does not implement energy management algorithms. It simply provides reliable communication between the EMS and field devices.

---

## 2. MQTT Controller

The MQTT Controller is intended for sites equipped with battery storage (BESS) and PCS.

Besides performing all Bridge functions, it also executes site-specific control algorithms.

Its responsibilities include:

* Receive power schedules from the EMS.
* Control PCS active power.
* Monitor battery State of Charge (SOC).
* Control PV inverter active_power_percent when required.
* Follow export/import limitations.
* Execute plant-specific optimisation algorithms.
* Publish telemetry and operational status.

Examples include:

* Rudnik Controller
* Boryana Controller
* Future hybrid PV + BESS controllers

Each controller may implement a different optimisation strategy while sharing the same communication framework.

---

# Project Architecture

The platform is organised into independent modules.

* **common/** – shared functionality (MQTT, Modbus, configuration, logging, utilities).
* **devices/** – hardware drivers for inverters, smart meters, BESS and PCS.
* **bridges/** – MQTT Bridge implementations.
* **controllers/** – MQTT Controller implementations.
* **legacy/** – archived production scripts kept for reference during migration.

This architecture allows new hardware vendors or control algorithms to be added with minimal changes to the existing codebase.

---

# Design Principles

* Modular architecture.
* Vendor-independent communication layer.
* Shared MQTT protocol across all devices.
* Reusable hardware drivers.
* Configuration via environment files (.env).
* Production-ready deployment using systemd services.
* High reliability and fault tolerance.
* Easy integration of new devices and algorithms.

---

# Supported Protocols

Current and planned communication protocols include:

* MQTT
* Modbus TCP
* Modbus RTU (RS-485)
* SunSpec Modbus
* Vendor-specific Modbus implementations

---

# Typical Deployment

A typical installation consists of:

```
                NetworxNRG EMS
                       │
                     MQTT
                       │
              NetworxNRG EMS Bridge
                       │
     ┌─────────────────┼─────────────────┐
     │                 │                 │
 PV Inverters     Smart Meter      PCS / BESS
```

The EMS communicates only through MQTT.

The Bridge or Controller handles all communication with field equipment.

---

# Project Goals

* One common platform for all supported hardware vendors.
* Standardised MQTT interface.
* Reusable device drivers.
* Separation between communication and control logic.
* Easy deployment on Orange Pi, Raspberry Pi and Linux-based industrial controllers.
* Simple addition of new devices and future control algorithms without modifying the existing architecture.

---

# License

This repository is intended for the development of the NetworxNRG EMS communication platform.
