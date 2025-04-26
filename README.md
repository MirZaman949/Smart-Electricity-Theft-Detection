#include <LiquidCrystal_I2C.h>
#include "config.h"

void setup() {
  // Initialize pins
  pinMode(RELAY_PIN, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, HIGH); // Relay OFF by default

  // Initialize LCD
  lcd.init();
  lcd.backlight();
  lcd.print("System Starting...");
  delay(2000);
  lcd.clear();
}

void loop() {
  // Read current from ACS712 sensor
  float current = readCurrent();
  // Read voltage (example: fixed 220V for simplicity)
  float voltage = 220.0; 

  // Calculate power
  float power = voltage * current;

  // Display values on LCD
  lcd.setCursor(0, 0);
  lcd.print("I:");
  lcd.print(current, 1);
  lcd.print("A  V:");
  lcd.print(voltage, 0);
  lcd.print("V");

  lcd.setCursor(0, 1);
  lcd.print("Power: ");
  lcd.print(power, 1);
  lcd.print("W");

  // Theft detection logic
  if (current < NORMAL_CURRENT_THRESHOLD && current > BYPASS_CURRENT_THRESHOLD) {
    triggerTheftAlert();
  }

  delay(1000); // Update every second
}

// Function to read current from ACS712
float readCurrent() {
  int sensorValue = analogRead(CURRENT_SENSOR_PIN);
  float voltage = (sensorValue / 1024.0) * 5.0; // Convert to voltage
  float current = (voltage - 2.5) / 0.185; // ACS712 sensitivity: 185mV/A
  return abs(current); // Return absolute value
}

// Theft alert action
void triggerTheftAlert() {
  lcd.clear();
  lcd.print("THEFT DETECTED!");
  digitalWrite(RELAY_PIN, LOW); // Cut power
  digitalWrite(BUZZER_PIN, HIGH); // Sound buzzer

  // Send SMS via GSM (pseudo-code - requires GSM library)
  // GSM.println("AT+CMGF=1");
  // GSM.println("AT+CMGS=\"+923331234567\"");
  // GSM.print("Alert: Theft detected. Current: ");
  // GSM.print(current);
  // GSM.println("A");

  delay(5000); // Hold alert for 5 seconds
  digitalWrite(BUZZER_PIN, LOW);
}
