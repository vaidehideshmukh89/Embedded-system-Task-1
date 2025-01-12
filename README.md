# Embedded-system-Task-#include <LiquidCrystal.h>

// Initialize LCD pins (if used)
LiquidCrystal lcd(7, 8, 9, 10, 11, 12); 

// Pin definitions
const int buttonPin = 2; // Push button pin
const int tempSensorPin = A0; // LM35 sensor pin

// Variables
int buttonState = 0;
int lastButtonState = 0;
int buttonCounter = 0;
float temperature = 0.0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Use internal pull-up resistor
  Serial.begin(9600); // Start serial communication

  // LCD setup (if used)
  lcd.begin(16, 2);
  lcd.print("Press Counter:");
}

void loop() {
  // Read button state
  buttonState = digitalRead(buttonPin);
  
  // Detect button press
  if (buttonState == LOW && lastButtonState == HIGH) { 
    delay(50); // Debounce
    buttonCounter++; 
  }
  lastButtonState = buttonState;
  
  // Read temperature from LM35
  int sensorValue = analogRead(tempSensorPin);
  temperature = (sensorValue * 5.0 / 1024.0) * 100.0; // Convert to Celsius
  
  // Display on serial monitor
  Serial.print("Button Count: ");
  Serial.print(buttonCounter);
  Serial.print(" | Temperature: ");
  Serial.print(temperature);
  Serial.println(" C");
  
  // Display on LCD (if used)
  lcd.setCursor(0, 1); // Move to second row
  lcd.print("Temp: ");
  lcd.print(temperature);
  lcd.print("C");
}
