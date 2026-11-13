
# Cost-Effective Smart Battery Management System

**3S3P Lithium-Ion Battery Pack | ESP32-S3 | Active Cell Balancing | Battery Monitoring & Protection**

## 1. Project Overview

This project focuses on the design and development of a cost-effective Battery Management System (BMS) for a 3S3P lithium-ion battery pack.

The system integrates battery-voltage monitoring, current sensing, temperature monitoring, active cell-balancing circuitry, protection hardware, and wireless data visualization using an ESP32-S3 microcontroller.

The objective is to explore an affordable battery-monitoring and protection solution for educational, Formula Student, and other low-voltage battery applications.

The project includes:

- Electronic circuit design and simulation
- Battery monitoring using an ESP32-S3
- Inductor-based active cell-balancing circuit design
- Voltage, current, and temperature sensing
- Comparator-based protection circuitry
- Web-based real-time monitoring
- Schematic design and PCB development
- Hardware prototyping and testing

> **Project Scope:** Educational low-voltage BMS prototype. This project has not been established as an automotive-grade or certified safety system.

---

## 2. Battery Pack Specifications

| Parameter | Specification |
|---|---|
| Battery Chemistry | Lithium-Ion |
| Cell Form Factor | 18650 |
| Battery Configuration | 3S3P |
| Number of Cells | 9 |
| Nominal Cell Voltage | 3.7 V |
| Nominal Pack Voltage | 11.1 V |
| Target Pack Capacity | 7.5 Ah |
| Target Cell Capacity | 2.5 Ah |
| Microcontroller | ESP32-S3-WROOM |
| Balancing Method | Inductor-Based Active Balancing |
| Monitoring Interface | Web Dashboard |

**Note:** The battery-pack calculations use 3.7 V, 2.5 Ah cells. Other sections of the project report contain different cell and pack specifications. The final physical battery rating requires confirmation.

---

## 3. Hardware Components

| Component | Function |
|---|---|
| ESP32-S3-WROOM | Data acquisition, processing and communication |
| ACS712 Current Sensor | Battery current monitoring |
| 10 kΩ NTC Thermistor | Temperature sensing circuit |
| DHT11 | Temperature sensing in documented firmware |
| Voltage Divider Network | Battery voltage measurement |
| AO3400/AO3401 MOSFETs | Cell-balancing switching |
| 120 µH / 220 µH Inductors | Energy transfer in balancing design |
| LM393 Comparator | Voltage-threshold comparison |
| BC547 Transistor | Relay driving |
| 5 V Relay | Load disconnection |
| 18650 Li-ion Cells | Battery pack |

---

## 4. System Architecture

The proposed Battery Management System consists of the following functional blocks:

1. Lithium-ion battery pack
2. Voltage monitoring circuitry
3. Current monitoring circuitry
4. Temperature monitoring circuitry
5. ESP32-S3 microcontroller
6. Active cell-balancing circuit
7. Protection and disconnection circuitry
8. Web-based monitoring interface

### Monitoring Data Flow

**Battery Pack → Sensing Circuits → ESP32-S3 → Wi-Fi → Web Dashboard**

The ESP32 receives sensor measurements, processes the acquired data, and transmits readings over Wi-Fi to a web interface.

The balancing and protection circuits are additional functional sections of the BMS design.

---

## 5. Battery Voltage Monitoring

Battery-voltage monitoring is implemented using resistor-divider circuits connected to ESP32 analog inputs.

### Voltage Divider Design

The documented circuit uses:

- R1 = 33 kΩ
- R2 = 10 kΩ

The voltage-divider relationship is:

**Vout = Vin × R2 / (R1 + R2)**

This scales measured voltages to a lower level suitable for the microcontroller's ADC input.

The firmware performs multiple ADC readings and uses averaging to reduce fluctuations.

### Engineering Considerations

- ADC input-voltage limitations
- Resistor tolerance
- Measurement noise
- ADC calibration
- Cell-group voltage calculation
- Safe voltage sensing

The final measurement accuracy must be verified against a reference instrument.

---

## 6. Current Monitoring

The design incorporates an **ACS712 Hall-effect current sensor**.

The project report specifies the ACS712 20 A variant for current monitoring.

### Purpose

- Monitor battery charging and discharging current
- Observe battery current consumption
- Support overcurrent detection
- Provide data for battery-system analysis

The sensor provides an analog output proportional to the measured current.

**Note:** Although the hardware design includes current monitoring, the firmware excerpt in the report primarily demonstrates voltage and temperature acquisition.

---

## 7. Temperature Monitoring

Temperature monitoring is included to help identify abnormal thermal conditions.

The report describes two temperature-sensing implementations.

### Thermistor-Based Circuit

The circuit design uses **10 kΩ NTC thermistors** connected through voltage-divider networks.

As temperature changes, thermistor resistance changes, producing a corresponding change in ADC input voltage.

### Firmware Implementation

The documented ESP32 firmware uses a **DHT11 temperature sensor** for temperature readings.

The final hardware configuration should be verified before publishing a complete temperature-sensing schematic or claiming measurement accuracy.

---

## 8. Active Cell Balancing

### Balancing Method

**Inductor-Based Active Cell Balancing**

Active balancing is intended to transfer energy between battery cell groups rather than dissipating the excess energy entirely as heat.

The proposed balancing circuit uses MOSFET switching and inductive energy storage.

### Major Components

- AO3400/AO3401 MOSFETs
- 120 µH and 220 µH inductors
- ESP32-based control architecture
- Battery cell-group connections

### Operating Principle

**Step 1: Voltage Measurement**

Battery cell-group voltages are measured to identify voltage imbalance.

**Step 2: Energy Storage**

MOSFET switching establishes an energy-transfer path through an inductor.

**Step 3: Energy Transfer**

The circuit is designed to redirect stored energy toward a lower-voltage group.

**Step 4: Balancing Control**

The intended switching sequence repeats to reduce voltage differences between cell groups.

### Validation Status

The report documents the active-balancing circuit architecture and schematic design.

However, quantitative experimental results for balancing current, transfer efficiency, and balancing time are not provided.

These measurements are necessary before claiming validated balancing performance.

---

## 9. Battery Protection System

The project investigates protection against abnormal battery operating conditions.

### Protection Functions

- Overvoltage protection
- Overcurrent protection
- Overtemperature protection
- Load disconnection

### Overvoltage Protection Circuit

The documented overvoltage protection design uses:

- LM393 comparator
- Resistor-divider reference
- BC547 transistor
- 5 V relay

### Operating Principle

1. A reference voltage is established using a resistor divider.
2. A monitored voltage is compared against the reference.
3. The comparator output controls a transistor.
4. The transistor drives a relay.
5. The relay provides a load-disconnection mechanism.

The report describes overcurrent and overtemperature protection as part of the system results.

### Testing Limitations

Complete experimental records for the following parameters are not included:

- Exact overvoltage trip threshold
- Exact overcurrent trip threshold
- Exact overtemperature trip threshold
- Protection response time
- Relay-disconnection timing

These require further documented validation.

---

## 10. ESP32 Firmware Development

The monitoring firmware was developed using the Arduino environment for the ESP32-S3.

### Documented Firmware Features

- Analog voltage sampling
- Multiple-sample ADC averaging
- Temperature measurement
- Wi-Fi communication
- HTTP web server
- JSON-based sensor data
- mDNS support
- OTA update initialization

### Monitoring Algorithm

1. Initialize the microcontroller.
2. Initialize serial communication.
3. Connect to Wi-Fi.
4. Initialize the temperature sensor.
5. Configure voltage-sensing inputs.
6. Start the web server.
7. Acquire sensor measurements.
8. Process voltage and temperature readings.
9. Generate JSON data.
10. Update the monitoring interface.
11. Repeat the acquisition process.

The included firmware demonstrates monitoring functionality. Separate validation is required for the balancing-control and protection functions described in the hardware architecture.

---

## 11. Web-Based Battery Monitoring

The project incorporates an ESP32-hosted web interface for remote monitoring.

### Documented Features

- Battery-voltage readings
- Temperature readings
- Web-based visualization
- JSON data transmission
- Wireless access over Wi-Fi

### Communication

The monitoring interface uses an HTTP server running on the ESP32.

The firmware defines:

- `/` — Web interface
- `/data` — JSON sensor measurements

This allows the browser interface to request updated battery information.

---

## 12. Circuit Design and Simulation

The project uses simulation and electronic design tools to investigate circuit behavior.

### Software Tools

| Software | Application |
|---|---|
| MATLAB Simulink | Battery-system modeling and simulation |
| Proteus | Circuit simulation |
| EasyEDA | Schematic and PCB design |
| Arduino IDE | ESP32 firmware development |

### Simulation Work

The project report includes:

- 3S3P battery-pack configuration
- Battery Management System simulation
- Current monitoring simulation
- Temperature monitoring simulation
- DC power supply simulation

Simulation results are documented separately from physical hardware test results.

---

## 13. PCB Design and Hardware Development

PCB schematics and layouts were developed using **EasyEDA**.

### Documented Design Work

- Battery monitoring schematic
- Voltage protection circuit schematic
- Active cell-balancing schematic
- Main BMS PCB layout
- Cell-balancing PCB layout

The project report also contains photographs of the physical BMS setup and PCB hardware.

### Development Stages

1. Component selection
2. Circuit design
3. Circuit simulation
4. Breadboard prototyping
5. Schematic development
6. PCB layout
7. Hardware assembly
8. Testing and observation

---

## 14. Project Results

The project report describes the development of a battery monitoring prototype with real-time web-based data visualization.

### Documented Outcomes

- Battery pack configuration design
- Battery-voltage measurement
- Temperature monitoring
- Current-sensing hardware selection
- ESP32 web interface development
- Active-balancing circuit design
- Protection circuit development
- PCB schematic and layout development
- Prototype hardware implementation

The report states that the system demonstrated protective behavior for overvoltage, overcurrent, and overtemperature conditions.

However, numerical fault-test results and complete performance measurements are not provided for every claimed function.

### Further Validation Required

- Cell-voltage measurement accuracy
- Current measurement accuracy
- Temperature measurement accuracy
- Active-balancing efficiency
- Balancing current and timing
- Fault-trigger thresholds
- Protection response time
- Long-duration reliability

---

## 15. Future Improvements

Potential improvements identified from the project and its limitations include:

- Dedicated battery-monitoring IC integration
- Improved voltage and temperature measurement calibration
- Detailed protection fault testing
- Active-balancing performance measurement
- Battery State of Charge estimation
- Battery State of Health estimation
- Data logging
- Improved monitoring dashboards
- More extensive hardware validation

These are proposed improvements, not implemented features.

---

## 16. Engineering Skills Demonstrated

This project involves the following engineering areas:

**Battery Systems**
- Lithium-ion battery configuration
- Battery monitoring
- Active-balancing circuit design
- Battery protection architecture

**Embedded Electronics**
- ESP32-S3 development
- ADC-based sensing
- Sensor interfacing
- Wi-Fi communication
- Web-based telemetry

**Hardware Development**
- Circuit simulation
- Electronic component selection
- Schematic capture
- PCB layout
- Hardware prototyping

**Engineering Practices**
- System architecture
- Hardware/software integration
- Measurement and testing
- Technical documentation
- Design verification

---

## 17. Project Team

This academic project was developed by:

- **Anshuman Choudhary**
- **Ragangi Alandkar**
- **Jay Sable**

**Institution:** Pune Vidyarthi Griha's College of Engineering, Pune

**Department:** Electronics and Telecommunication Engineering

**University:** Savitribai Phule Pune University

The project is a collaborative academic engineering effort. Individual contributions should be documented separately where appropriate.

---

## 18. Safety Disclaimer

**This is an educational battery-electronics prototype and not a certified commercial Battery Management System.**

The system should not be relied upon as the only protection device for an electric vehicle traction battery or any other safety-critical battery application.

Lithium-ion batteries can present electrical, thermal, and fire hazards. Proper engineering validation and independent protection mechanisms are essential.

---

## 19. Author

**Anshuman Choudhary**

Electronics & Telecommunication Engineering

Savitribai Phule Pune University, Pune, India

**GitHub:** [AnshumanChoudhary529](https://github.com/AnshumanChoudhary529)

**LinkedIn:** [Anshuman Choudhary](https://www.linkedin.com/in/anshuman--choudhary/)

---

**Project Category:** Embedded Systems | Battery Electronics | PCB Design | Battery Management Systems
