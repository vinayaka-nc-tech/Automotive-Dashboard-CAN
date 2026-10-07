# Automotive Dashboard Using CAN Protocol

A multi-ECU automotive dashboard project developed using three PIC18F4580 microcontrollers. The ECUs communicate with each other using the CAN (Controller Area Network) protocol to collect and display vehicle parameters such as speed, gear, RPM, and indicator status.

## Project Overview

The system is divided into three independent ECUs, where each ECU performs a specific task.

- **ECU 1:** Processes vehicle speed and gear selection.
- **ECU 2:** Processes engine RPM and indicator control.
- **ECU 3:** Receives data from ECU 1 and ECU 2 through CAN and displays the information on a 16x2 CLCD.

This architecture demonstrates how multiple embedded controllers can communicate over a common CAN bus while performing independent tasks.

## System Architecture

```text
                    CAN BUS
        ┌─────────────────────────────┐
        │                             │
        ▼                             ▼
┌─────────────────┐           ┌─────────────────┐
│      ECU 1      │           │      ECU 2      │
│ Speed & Gear    │           │ RPM & Indicator │
│                 │           │                 │
│ Potentiometer   │           │ Potentiometer   │
│      ↓          │           │      ↓          │
│    Speed        │           │     RPM         │
│                 │           │                 │
│ Keypad          │           │ Digital Keypad  │
│      ↓          │           │      ↓          │
│     Gear        │           │   Indicator     │
└────────┬────────┘           └────────┬────────┘
         │                             │
         └──────────────┬──────────────┘
                        │
                        ▼
                ┌─────────────────┐
                │      ECU 3      │
                │  Data Receiver  │
                │       +         │
                │  CLCD Display   │
                └─────────────────┘
                ECU 1 - Speed and Gear

ECU 1 is responsible for processing the vehicle speed and gear information.

Speed

A potentiometer is connected to the ADC input to simulate the vehicle speed.

The ADC value is converted into a speed value and transmitted through CAN using:

CAN ID: 0x10
Gear

A keypad is used to select the gear.

The available gear states are:

GO → GN → G1 → G2 → G3 → G4 → G5 → G6

The selected gear is transmitted through CAN using:

CAN ID: 0x20
ECU 2 - RPM and Indicator

ECU 2 handles engine RPM and indicator control.

RPM

A potentiometer is connected to the ADC input to simulate the engine RPM.

The ADC value is converted into RPM and transmitted through CAN using:

CAN ID: 0x30
Indicators

A digital keypad is used to control the indicators.

The system supports:

Left Indicator
Right Indicator
Indicator OFF

The indicator status is transmitted using:

CAN ID: 0x50

The indicator LEDs are also controlled according to the selected direction.

ECU 3 - Dashboard Display

ECU 3 acts as the central display ECU.

It receives CAN messages from ECU 1 and ECU 2 and displays the received information on a 16x2 CLCD.

The display contains:

S:   G:   R:   I:

where:

S → Speed
G → Gear
R → RPM
I → Indicator status

The indicator direction is represented on the display using:

<-    Left
->    Right
CAN Communication

The ECUs communicate using the built-in CAN controller of the PIC18F4580.

CAN Message IDs
Parameter	CAN ID	Source ECU	Destination
Speed	0x10	ECU 1	ECU 3
Gear	0x20	ECU 1	ECU 3
RPM	0x30	ECU 2	ECU 3
Indicator	0x50	ECU 2	ECU 3

Standard CAN message identifiers are used for communication.

Hardware Used
3 × PIC18F4580 microcontrollers
Potentiometer for speed input
Potentiometer for RPM input
Keypad for gear selection
Digital keypad for indicator control
16x2 CLCD
Indicator LEDs
CAN communication interface
Software and Tools
Embedded C
PIC18F4580
MPLAB X IDE
XC8 Compiler
CAN Protocol
ADC
16x2 CLCD
Git & GitHub
Project Structure
Automotive-Dashboard-CAN/
│
├── ECU_1_Speed_Gear/
│   └── ECU__1.X/
│       ├── adc.c
│       ├── adc.h
│       ├── can.c
│       ├── can.h
│       ├── clcd.c
│       ├── clcd.h
│       ├── digital_keypad.c
│       ├── digital_keypad.h
│       ├── main.c
│       ├── main.h
│       ├── msg_id.h
│       └── nbproject/
│
├── ECU2_2_RPM_Indicator/
│   └── ECU__2.X/
│       ├── adc.c
│       ├── adc.h
│       ├── can.c
│       ├── can.h
│       ├── digital_keypad.c
│       ├── digital_keypad.h
│       ├── main.c
│       ├── main.h
│       ├── msg_id.h
│       ├── ssd.c
│       ├── ssd.h
│       └── nbproject/
│
├── ECU_3_Display/
│   └── ECU__3.X/
│       ├── can.c
│       ├── can.h
│       ├── clcd.c
│       ├── clcd.h
│       ├── main.c
│       ├── main.h
│       ├── msg_id.h
│       └── nbproject/
│
├── .gitignore
└── README.md
Working Flow
ECU 1 reads the potentiometer value and calculates the vehicle speed.
ECU 1 reads the keypad input and determines the selected gear.
ECU 1 transmits speed and gear information through CAN.
ECU 2 reads the potentiometer and calculates RPM.
ECU 2 reads the indicator input and determines the indicator state.
ECU 2 transmits RPM and indicator information through CAN.
ECU 3 continuously receives CAN messages.
ECU 3 identifies each message using its CAN ID.
The received values are displayed on the 16x2 CLCD.
Indicator status is also represented using the corresponding LEDs.
Key Features
Three independent ECU architecture
CAN-based ECU-to-ECU communication
ADC-based speed and RPM simulation
Keypad-based gear selection
Digital keypad-based indicator control
Real-time CLCD dashboard display
Indicator LED control
Modular Embedded C implementation
Future Improvements
Add engine temperature monitoring
Add vehicle warning indicators
Improve CAN message filtering
Add timer/interrupt based periodic communication
Add a more detailed dashboard display
Replace potentiometer inputs with actual automotive sensors
Author

Vinayaka NC

Electronics and Communication Engineering
The National Institute of Engineering, Mysore
```
