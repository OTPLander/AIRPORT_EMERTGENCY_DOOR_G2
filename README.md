# Airport Emergency Door Automation System

## Project Overview
This project implements an automated control system for a high-security airport emergency door with a HMI using the **Beckhoff TwinCAT 3** environment. The system integrates industrial PLC logic with an external Raspberry Pi (RPi) for biometric facial detection and communication monitoring.

The core logic is based on a GEMMA-inspired state machine, ensuring deterministic behavior during normal production, manual configuration, and failure recovery procedures.

## System Architecture

### Hardware & Communication
* **PLC Hardware**: Beckhoff EK1100 (EtherCAT Coupler), EL2008 (Digital Outputs), EL2100 (Digital Inputs).
* **Peripheral Controller**: Raspberry Pi (RPi) interfaced via **OPC UA**.
* **Protocols**: 
  - **EtherCAT**: Real-time I/O management for physical switches and actuators.
  - **OPC UA**: Data exchange for image buffers and security handshakes.

### Key Components (POUs)
* **`MAIN`**: Orchestrates high-level logic, initializes OPC UA buffers, and executes the system watchdog.
* **`FB_EMERGENCY_SYS`**: The primary state controller managing 16 distinct states and 37 transitions. It handles the door's opening/closing logic and alarm triggers.
* **`FB_RPi_WATCHDOG`**: A safety monitor that tracks an integer-based heartbeat from the Raspberry Pi. It triggers a failure state if communication is lost for more than 3 seconds.
* **`PWM`**: Generates a Pulse Width Modulation signal (50Hz) for variable intensity control of alarm indicators.
* ** `FB_STEPPER_CTRL` **: Generates the pulses that control the stepper motor.
* **`FB_HMI_CTRL` **: Manages HMI image background (changes depending on the state). 


## Logic & State Machine
The system operates through several procedural blocks:
1. **A - Start/Stop Procedures**: Management of the `stDisconnected` and `stClosing` states.
2. **F - Production Procedures**: Real-time monitoring of the RPi connection (`stRaspberryOk`) and standby modes.
3. **D - Failure Procedures**: Automatic emergency opening if the RPi watchdog fails or hardware errors are detected.
4. **Testing Mode**: Manual bypass of presence sensors for maintenance and tuning.


## Technical Specifications
* **State Management**: Evaluated via a centralized `sysType` structure.
* **Inputs**: Physical buttons (Start, Alarm, Test) and sensors (Reed Switch, Facial Detection).
* **Outputs**: Direct control for `oOpenDoor`, `oCloseDoor`, and `oAlarm`.
* **Security**: Multi-stage alarm handling including "Normal Alarm" and "Terrorist/Facial Detection Alarm".

## Development Team
* **Iñigo Bañuls**
* **Lander Nieto**
* **Ruud Rohan**

*Date: April 2026*
