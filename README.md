# spinninglidar
A rotating lidar sensor using 4 VL53L0X sensors


Spinning LIDAR System - Complete Wiring Guide
ULN2003 Driver with 6-Wire Slip Ring

Components List
Stationary Base Section

Arduino Mega 2560 (base controller)
ULN2003 Stepper Driver Board (4 inputs: IN1-IN4)
28BYJ-48 5V Stepper Motor
6-Channel Slip Ring (stationary side)
12V Power Supply (or 5V if only using motor)
5V Buck Converter (if using 12V supply)
Base plate (wood/acrylic)

Rotating Platform Section

Arduino Pro Mini 5V 16MHz (rotating controller)
4× VL53L0X ToF Sensors
Hall Effect Sensor (A3144 or similar)
6-Channel Slip Ring (rotating side)
Small prototyping board
Circular platform (150mm diameter)

Additional Hardware

Small neodymium magnet (for hall sensor trigger)
Jumper wires (various lengths)
USB cable (for programming Arduinos)
Standoffs/mounts for sensors


Wiring Diagram
Base Arduino Mega 2560 Connections
┌─────────────────────────────────────────┐
│        ARDUINO MEGA 2560 (BASE)         │
├─────────────────────────────────────────┤
│                                         │
│  Digital Pins:                          │
│    Pin 2  → Hall Sensor (via slip ring) │
│    Pin 8  → ULN2003 IN1                │
│    Pin 9  → ULN2003 IN2                │
│    Pin 10 → ULN2003 IN3                │
│    Pin 11 → ULN2003 IN4                │
│    Pin 12 → INIT Signal (via slip ring) │
│                                         │
│  I2C Pins:                              │
│    SDA (20) → Slip Ring Ch3             │
│    SCL (21) → Slip Ring Ch4             │
│                                         │
│  Power:                                 │
│    5V → Slip Ring Ch1 (to rotating)     │
│    GND → Slip Ring Ch2 (common ground)  │
│                                         │
└─────────────────────────────────────────┘
ULN2003 Driver Board Connections
┌─────────────────────────────────────────┐
│         ULN2003 DRIVER BOARD            │
├─────────────────────────────────────────┤
│                                         │
│  Inputs (from Arduino Mega):            │
│    IN1 → Pin 8                          │
│    IN2 → Pin 9                          │
│    IN3 → Pin 10                         │
│    IN4 → Pin 11                         │
│                                         │
│  Power:                                 │
│    VCC → 5V                             │
│    GND → GND                            │
│                                         │
│  Motor Output:                          │
│    OUT1 → 28BYJ-48 Coil 1              │
│    OUT2 → 28BYJ-48 Coil 2              │
│    OUT3 → 28BYJ-48 Coil 3              │
│    OUT4 → 28BYJ-48 Coil 4              │
│         (via 5-pin connector)           │
│                                         │
└─────────────────────────────────────────┘
6-Wire Slip Ring Connections
┌─────────────────────────────────────────────────────────────┐
│                    SLIP RING (6 CHANNELS)                   │
├──────────────────────┬──────────────────────────────────────┤
│   STATIONARY SIDE    │         ROTATING SIDE                │
├──────────────────────┼──────────────────────────────────────┤
│                      │                                      │
│  Ch1: +5V Power ─────┼────→ Arduino Pro Mini VCC           │
│  Ch2: GND ───────────┼────→ Arduino Pro Mini GND           │
│  Ch3: I2C SDA ───────┼────→ Arduino Pro Mini SDA (A4)      │
│  Ch4: I2C SCL ───────┼────→ Arduino Pro Mini SCL (A5)      │
│  Ch5: INIT Signal ───┼────→ Arduino Pro Mini Pin 2         │
│  Ch6: Hall Sensor ←──┼──── Hall Sensor Signal              │
│                      │                                      │
└──────────────────────┴──────────────────────────────────────┘
Rotating Platform - Arduino Pro Mini Connections
┌─────────────────────────────────────────┐
│      ARDUINO PRO MINI (ROTATING)        │
├─────────────────────────────────────────┤
│                                         │
│  Power (from slip ring):                │
│    VCC → Slip Ring Ch1 (+5V)            │
│    GND → Slip Ring Ch2 (GND)            │
│                                         │
│  Digital Pins:                          │
│    Pin 2 → INIT Signal (from base)      │
│    Pin 3 → Hall Sensor Output           │
│    Pin 6 → VL53L0X #0 XSHUT             │
│    Pin 7 → VL53L0X #1 XSHUT             │
│    Pin 8 → VL53L0X #2 XSHUT             │
│    Pin 9 → VL53L0X #3 XSHUT             │
│    Pin 13 → Status LED (onboard)        │
│                                         │
│  I2C Pins:                              │
│    A4 (SDA) → All VL53L0X SDA pins      │
│    A5 (SCL) → All VL53L0X SCL pins      │
│                                         │
└─────────────────────────────────────────┘
VL53L0X Sensor Connections (All 4 Sensors)
┌─────────────────────────────────────────┐
│           VL53L0X SENSORS               │
│              (×4 sensors)               │
├─────────────────────────────────────────┤
│                                         │
│  Sensor #0 (0° position):               │
│    VCC → 5V (from Pro Mini)             │
│    GND → GND                            │
│    SDA → Pro Mini A4 (shared)           │
│    SCL → Pro Mini A5 (shared)           │
│    XSHUT → Pro Mini Pin 6               │
│    GPIO1 → Not connected                │
│                                         │
│  Sensor #1 (90° position):              │
│    VCC → 5V                             │
│    GND → GND                            │
│    SDA → Pro Mini A4 (shared)           │
│    SCL → Pro Mini A5 (shared)           │
│    XSHUT → Pro Mini Pin 7               │
│    GPIO1 → Not connected                │
│                                         │
│  Sensor #2 (180° position):             │
│    VCC → 5V                             │
│    GND → GND                            │
│    SDA → Pro Mini A4 (shared)           │
│    SCL → Pro Mini A5 (shared)           │
│    XSHUT → Pro Mini Pin 8               │
│    GPIO1 → Not connected                │
│                                         │
│  Sensor #3 (270° position):             │
│    VCC → 5V                             │
│    GND → GND                            │
│    SDA → Pro Mini A4 (shared)           │
│    SCL → Pro Mini A5 (shared)           │
│    XSHUT → Pro Mini Pin 9               │
│    GPIO1 → Not connected                │
│                                         │
└─────────────────────────────────────────┘
Hall Effect Sensor Connections
┌─────────────────────────────────────────┐
│        HALL EFFECT SENSOR               │
│          (A3144 or similar)             │
├─────────────────────────────────────────┤
│                                         │
│  Sensor positioned on rotating platform │
│  Magnet positioned on stationary base   │
│                                         │
│  Pin 1 (VCC) → 5V (Pro Mini)            │
│  Pin 2 (GND) → GND (Pro Mini)           │
│  Pin 3 (OUT) → Pro Mini Pin 3           │
│              → Slip Ring Ch6            │
│              → Base Arduino Pin 2       │
│                                         │
│  Add 10kΩ pull-up resistor:             │
│    OUT → 5V (on rotating side)          │
│                                         │
└─────────────────────────────────────────┘

Power Distribution
Power Budget
ComponentQuantityCurrent EachTotal CurrentVL53L0X Sensors419mA76mAArduino Pro Mini120mA20mAHall Sensor110mA10mARotating Total--~106mAArduino Mega (Base)150mA50mAULN2003 Driver130mA30mA28BYJ-48 Motor1200mA200mABase Total--~280mASystem Total--~386mA
Power Supply Options
Option 1: Single 5V Supply (Recommended)

5V 2A wall adapter
Powers entire system
Simple and clean

Option 2: Dual Voltage

12V supply for motor
5V buck converter for electronics
More power headroom


Assembly Instructions
Step 1: Build Stationary Base

Mount Arduino Mega on base plate
Mount ULN2003 driver board near motor
Mount stepper motor vertically
Attach slip ring stationary side to base
Attach small magnet to base (in motor rotation path)

Step 2: Build Rotating Platform

Cut circular platform (150mm diameter)
Mount Arduino Pro Mini at center
Mount 4× VL53L0X sensors at 0°, 90°, 180°, 270°
Mount hall sensor at edge (to pass by magnet)
Attach slip ring rotating side to platform center
Couple platform to motor shaft

Step 3: Wiring Base Section

Connect ULN2003 to Arduino Mega (pins 8-11)
Connect motor to ULN2003 (5-pin connector)
Wire slip ring channels 1-5 to Arduino
Connect hall sensor wire from slip ring to pin 2
Connect power supply

Step 4: Wiring Rotating Section

Wire Arduino Pro Mini to slip ring (power + I2C)
Connect all VL53L0X sensors (shared I2C bus)
Wire XSHUT pins individually (pins 6-9)
Connect hall sensor to Pro Mini pin 3 and slip ring
Add 10kΩ pull-up on hall sensor output

Step 5: Programming

Program Arduino Pro Mini with rotating_init_circuit.ino
Program Arduino Mega with lidar_base_6wire.ino
Test each section independently first
Combine and test full system


Testing Checklist

 Base Arduino powers on
 Motor rotates smoothly (use stepper test script)
 Slip ring channels have continuity
 Rotating Arduino receives power through slip ring
 All 4 VL53L0X sensors detected (check Serial monitor)
 Hall sensor triggers once per revolution
 I2C communication works through slip ring
 Motor maintains constant speed (60 RPM)
 Distance data streams continuously
 No excessive noise or jitter in readings


Troubleshooting
Motor doesn't rotate

Check ULN2003 wiring (IN1-IN4)
Verify motor power (5V to ULN2003)
Test with stepper_uln2003_test.ino first

Sensors not detected

Check I2C wiring through slip ring
Verify 5V power to rotating section
Test rotating Arduino independently
Check for loose slip ring connections

Noisy I2C data

Add 100nF capacitors on I2C lines
Shorten wire lengths where possible
Use twisted pair for SDA/SCL through slip ring
Lower I2C speed (400kHz → 100kHz)

Hall sensor not triggering

Check magnet position and strength
Verify pull-up resistor (10kΩ)
Test hall sensor independently
Adjust magnet distance (2-5mm optimal)


Notes

Keep slip ring wires as short as practical
Use color-coded wires for easier troubleshooting
Add small capacitor (10µF) near rotating Arduino VCC/GND
Consider adding status LEDs on rotating platform
Balance the rotating platform to reduce vibration
Secure all wires on rotating platform to prevent snagging
