# Dual Compartment Micro Farm Automation System

[![Arduino](https://img.shields.io/badge/Arduino-Compatible-blue.svg)](https://www.arduino.cc/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.0-orange.svg)]()

An intelligent Arduino-based automation system for controlling two separate micro farm compartments with independent environmental monitoring and control.

## Table of Contents

- [Features](#-features)
- [Hardware Requirements](#-hardware-requirements)
- [Circuit Diagram](#-circuit-diagram)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [Compartment Settings](#-compartment-settings)
- [Pin Mapping](#-pin-mapping)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

## Features

### Dual Compartment Control
- **Independent Environmental Monitoring**: Separate DHT11 sensors for each compartment
- **Isolated Control Systems**: Each compartment operates independently with its own relay controls
- **Customizable Settings**: Different temperature, humidity, and lighting schedules per compartment

###  Environmental Control
- **Temperature Management**: Automatic cooling fan control with configurable thresholds
- **Humidity Control**: Dehumidifier activation with hysteresis to prevent oscillation
- **Smart Alerts**: Low humidity warnings for optimal plant growth

### Automated Irrigation
- **Scheduled Watering**: Dual daily watering cycles (morning and evening)
- **Smart Watering Logic**: Skips watering if humidity is too high
- **Customizable Duration**: Different watering times per compartment

### Lighting Management
- **Automated Photoperiods**: Configurable grow light schedules
- **Individual Light Cycles**: Different lighting schedules for each compartment
- **Energy Efficient**: Automatic on/off based on time

### Advanced Features
- **Hysteresis Control**: Prevents relay chattering and extends equipment life
- **Serial Monitoring**: Real-time status updates and debugging
- **Emergency Stop**: Safety function to disable all systems instantly
- **Comprehensive Logging**: Detailed system status reports

##  Hardware Requirements

### Core Components
- **Arduino Mini** (or Arduino Uno/Nano)
- **2x DHT11** Digital Temperature & Humidity Sensors
- **2x 4-Channel Relay Modules** (5V, optically isolated)
- **12V Power Supply** (2A minimum)
- **5V Regulator** (if using 12V input)

### Optional Components
- **DS3231 RTC Module** (for accurate timekeeping)
- **LCD Display** (16x2 I2C)
- **Emergency Stop Button**
- **Water Level Sensors**
- **Soil Moisture Sensors**

### Load Devices (Per Compartment)
- **Cooling Fan** (12V DC or 220V AC)
- **Dehumidifier/Exhaust Fan** 
- **Water Pump** (12V DC submersible)
- **LED Grow Lights** (12V LED strips or AC grow lights)

## 🔌 Circuit Diagram

![Circuit Diagram](circuit-diagram.svg)

### Pin Mapping

| Arduino Pin | Component | Function |
|-------------|-----------|----------|
| D2 | DHT11 A | Temperature/Humidity Sensor (Compartment A) |
| D3 | DHT11 B | Temperature/Humidity Sensor (Compartment B) |
| D4 | Relay A1 | Cooling Fan (Compartment A) |
| D5 | Relay A2 | Dehumidifier (Compartment A) |
| D6 | Relay A3 | Water Pump (Compartment A) |
| D7 | Relay A4 | Grow Lights (Compartment A) |
| D8 | Relay B1 | Cooling Fan (Compartment B) |
| D9 | Relay B2 | Dehumidifier (Compartment B) |
| D10 | Relay B3 | Water Pump (Compartment B) |
| D11 | Relay B4 | Grow Lights (Compartment B) |
| VCC | 5V | Power for Arduino and sensors |
| GND | Ground | Common ground |

## Installation

### 1. Hardware Setup
1. Connect the DHT11 sensors to pins D2 and D3
2. Wire the relay modules to pins D4-D11 as per the pin mapping
3. Connect power supply (12V) with 5V regulation for Arduino
4. Ensure proper grounding of all components

### 2. Software Installation
1. Install Arduino IDE from [arduino.cc](https://www.arduino.cc/en/software)
2. Install the DHT sensor library:
   ```
   Tools → Manage Libraries → Search "DHT sensor library" → Install
   ```
3. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/dual-compartment-micro-farm.git
   ```
4. Open `dual_compartment_micro_farm.ino` in Arduino IDE
5. Select your Arduino board and port
6. Upload the code

### 3. Library Dependencies
```cpp
#include <DHT.h>  // DHT sensor library by Adafruit
```

## Configuration

### Compartment Settings Structure
```cpp
struct CompartmentSettings {
  float tempThreshold;        // Temperature threshold (°C)
  float humidityThreshold;    // Humidity threshold (%)
  float lowHumidityThreshold; // Low humidity warning (%)
  int lightOnHour;           // Light on time (24h format)
  int lightOffHour;          // Light off time (24h format)
  int morningWaterHour;      // Morning watering time
  int eveningWaterHour;      // Evening watering time
  unsigned long wateringDuration; // Watering duration (ms)
};
```

### Default Configuration

#### Compartment A (Seedlings/Herbs)
```cpp
CompartmentSettings settingsA = {
  23.0,  // Temperature threshold: 23°C
  70.0,  // Humidity threshold: 70%
  40.0,  // Low humidity warning: 40%
  6,     // Lights on: 6 AM
  20,    // Lights off: 8 PM
  7,     // Morning watering: 7 AM
  18,    // Evening watering: 6 PM
  30000  // Watering duration: 30 seconds
};
```

#### Compartment B (Mature Plants)
```cpp
CompartmentSettings settingsB = {
  25.0,  // Temperature threshold: 25°C
  65.0,  // Humidity threshold: 65%
  35.0,  // Low humidity warning: 35%
  7,     // Lights on: 7 AM
  19,    // Lights off: 7 PM
  8,     // Morning watering: 8 AM
  17,    // Evening watering: 5 PM
  45000  // Watering duration: 45 seconds
};
```

##  Usage

### 1. Power On
- Connect power supply and turn on the system
- Open Serial Monitor (9600 baud) to view system status
- System will initialize and begin monitoring

### 2. Monitoring
- Real-time sensor readings displayed every 2 seconds
- System status updates when relay states change
- Environmental alerts for optimal growing conditions

### 3. Manual Control
- Use the `emergencyStop()` function to disable all systems
- Modify thresholds in code and re-upload for different crops
- Monitor serial output for troubleshooting

### Serial Output Example
```
 DUAL COMPARTMENT MICRO FARM STATUS 
========================================
Time: 14:00
----------------------------------------
📦 COMPARTMENT A:
   Temperature: 24.5°C (Threshold: 23.0°C)
   Humidity: 68% (Threshold: 70%)
   Cooling Fan: ON
   Dehumidifier: OFF
   Water Pump: OFF
   Grow Lights: ON
----------------------------------------
📦 COMPARTMENT B:
   Temperature: 22.1°C (Threshold: 25.0°C)
   Humidity: 63% (Threshold: 65%)
   Cooling Fan: OFF
   Dehumidifier: OFF
   Water Pump: OFF
   Grow Lights: ON
========================================
```

## Compartment Settings

### Recommended Settings by Plant Type

#### Herbs (Basil, Mint, Parsley)
- **Temperature**: 20-24°C
- **Humidity**: 60-70%
- **Light**: 14-16 hours
- **Watering**: Light, frequent

#### Leafy Greens (Lettuce, Spinach, Kale)
- **Temperature**: 18-22°C
- **Humidity**: 50-70%
- **Light**: 12-14 hours
- **Watering**: Moderate, consistent

#### Fruiting Plants (Tomatoes, Peppers)
- **Temperature**: 22-26°C
- **Humidity**: 50-60%
- **Light**: 14-16 hours
- **Watering**: Deep, less frequent

####  Seedlings
- **Temperature**: 20-25°C
- **Humidity**: 70-80%
- **Light**: 12-14 hours
- **Watering**: Light misting

## 🔧 Troubleshooting

### Common Issues

#### Sensor Readings Show NaN
- **Cause**: Loose connections or faulty DHT11 sensor
- **Solution**: Check wiring, ensure 3.3-5V power supply

#### Relays Not Switching
- **Cause**: Insufficient power or incorrect wiring
- **Solution**: Verify 5V supply to relay modules, check signal connections

#### Inconsistent Watering
- **Cause**: Time simulation in demo code
- **Solution**: Replace time simulation with RTC module for production use

#### High Power Consumption
- **Cause**: All loads running simultaneously
- **Solution**: Implement load balancing or staggered operation

### Diagnostic Commands
```cpp
// Add these functions for debugging
void printSensorRaw() {
  // Print raw sensor values
}

void testRelays() {
  // Cycle through all relays for testing
}

void checkConnections() {
  // Verify all pin connections
}
```

## Upgrades and Modifications

### Recommended Enhancements

1. **Real-Time Clock (RTC)**
   ```cpp
   #include <RTClib.h>
   RTC_DS3231 rtc;
   ```

2. **LCD Display**
   ```cpp
   #include <LiquidCrystal_I2C.h>
   LiquidCrystal_I2C lcd(0x27, 16, 2);
   ```

3. **WiFi Connectivity**
   ```cpp
   #include <ESP8266WiFi.h>
   // Remote monitoring and control
   ```

4. **Data Logging**
   ```cpp
   #include <SD.h>
   // Log sensor data to SD card
   ```

##  Safety Considerations

### Electrical Safety
- Use appropriate fuses for AC loads
- Ensure proper grounding of all equipment
- Keep electrical connections away from water
- Use GFCI outlets for AC-powered devices

### Water Safety
- Use IP65 rated enclosures near water
- Install overflow protection
- Use food-grade tubing for water systems
- Regular maintenance of pumps and filters

### System Safety
- Implement watchdog timer for system reliability
- Add temperature failsafes to prevent overheating
- Include manual override switches
- Regular calibration of sensors

## Performance Monitoring

### Key Metrics to Track
- **Temperature Stability**: ±1°C of setpoint
- **Humidity Control**: ±5% of setpoint
- **Power Consumption**: Monitor for efficiency
- **Watering Accuracy**: Consistent timing and duration
- **System Uptime**: Reliability tracking

##  Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines
- Follow Arduino coding standards
- Add comments for complex logic
- Test all hardware configurations
- Update documentation for new features



## 🙏 Acknowledgments

- [Adafruit](https://adafruit.com) for the DHT sensor library
- Arduino community for extensive documentation
- Contributors and testers who helped improve this project



---

**Happy Growing! 🌱**

*Made with ❤️ for the indoor farming community*
