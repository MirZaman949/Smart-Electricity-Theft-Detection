##ifndef CONFIG_H
#define CONFIG_H

// Sensor Pins
#define CURRENT_SENSOR_PIN A0    // ACS712 connected to analog pin A0
#define VOLTAGE_SENSOR_PIN A1    // Voltage sensor on A1

// Relay and Buzzer Pins
#define RELAY_PIN 7              // Relay control pin
#define BUZZER_PIN 8             // Buzzer for alerts

// LCD Configuration (I2C)
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27, 16, 2); // I2C address 0x27, 16x2 display

// Thresholds
#define N
