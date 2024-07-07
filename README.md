# SMART-PARKING-SYSTEM
## Project Description

### Overview
This project involves creating a smart parking system using an Arduino Uno R3. The system utilizes infrared (IR) sensors to detect the presence of vehicles in parking slots, an LCD to display slot availability, and servo motors to control barriers at the entry and exit points. The project aims to automate the process of vehicle parking, making it efficient and user-friendly.

### Components
- **Arduino Uno R3**: The microcontroller used to control the system.
- **LCD 16x2**: Displays the status of the parking slots.
- **Potentiometer**: Adjusts the contrast of the LCD.
- **Resistor (220 Ω)**: Limits the current to the LCD.
- **PIR Sensors**: Detects the presence of vehicles.
- **Servo Motors**: Controls the barriers at the entry and exit points.

### Procedure
1. **Hardware Setup**:
    - Connect the LCD to the Arduino using appropriate pins.
    - Connect the potentiometer to adjust the LCD contrast.
    - Connect resistors to limit the current to the LCD.
    - Connect the IR sensors to detect the presence of vehicles in the parking slots and at the entry/exit points.
    - Connect the servo motors to control the barriers.

2. **Software Setup**:
    - Initialize the LCD and servo motors.
    - Set the IR sensor pins as input.
    - Use the loop function to continuously check the status of the IR sensors.
    - Update the LCD with the status of the parking slots.
    - Control the servo motors based on the sensor inputs.

### Code Explanation

```cpp
#include <Adafruit_LiquidCrystal.h>
#include <LiquidCrystal.h>
#include <Servo.h>

// Define servo objects
Servo S1, S2;

// Define pins for IR sensors and servos
#define IR_Slot1 7
#define IR_Slot2 8
#define IR_entry 6
#define IR_exit 13

int pos = 0; // Initial position of servo motors

// Initialize the library with the numbers of the interface pins
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  S1.attach(10); // Attach servo 1 to pin 10
  S2.attach(9);  // Attach servo 2 to pin 9

  S1.write(pos); // Initial position at 0 for both motors
  S2.write(pos);

  pinMode(IR_Slot1, INPUT); // Set IR sensor pins as input
  pinMode(IR_Slot2, INPUT); 
  pinMode(IR_entry, INPUT);
  pinMode(IR_exit, INPUT);

  lcd.begin(16, 2); // Initialize the LCD

  // Display initial message
  lcd.print(" Smart Parking");
  lcd.setCursor(0, 1);
  lcd.print("    System");
  delay(2000);
  lcd.clear();

  // Display initial slot status
  lcd.setCursor(0, 0);
  lcd.print("Slot 1 = A"); 
  lcd.setCursor(0, 1);
  lcd.print("Slot 2 = A");
  delay(2000);
}

void loop() {
  // Check the status of slot 1
  if (digitalRead(IR_Slot1) == HIGH) {
    lcd.setCursor(0, 0);
    lcd.print("Slot 1 = NA");
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Slot 1 = A ");
  }

  // Check the status of slot 2
  if (digitalRead(IR_Slot2) == HIGH) {
    lcd.setCursor(0, 1);
    lcd.print("Slot 2 = NA");
  } else {
    lcd.setCursor(0, 1);
    lcd.print("Slot 2 = A ");
  }

  // Control entry barrier
  if (digitalRead(IR_entry) == HIGH) {
    S1.write(pos + 90); // Open the entry barrier
  } else {
    S1.write(pos); // Close the entry barrier
  }

  // Control exit barrier
  if (digitalRead(IR_exit) == HIGH) {
    S2.write(pos + 90); // Open the exit barrier
  } else {
    S2.write(pos); // Close the exit barrier
  }
}
```

### Code Breakdown
- **Library Inclusions**: The necessary libraries for controlling the LCD and servo motors are included.
- **Servo Objects**: `S1` and `S2` are created to control the entry and exit barriers.
- **Pin Definitions**: The pins for the IR sensors and servos are defined.
- **Setup Function**:
  - The servos are attached to their respective pins and initialized to their starting positions.
  - The IR sensor pins are set as input.
  - The LCD is initialized, and a welcome message is displayed.
- **Loop Function**:
  - The status of the parking slots is checked using the IR sensors.
  - The LCD is updated with the status of the parking slots.
  - The entry and exit barriers are controlled based on the IR sensor inputs.

