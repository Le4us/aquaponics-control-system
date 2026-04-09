***Aquaponics Control System Group 16 - Arduino Code***



============================================

Makeshift/proof of concept pH sensor (prototype) CONTROL CODE

============================================



\#include <Adafruit\_LiquidCrystal.h>



// Initialize LCD (I2C address default)

Adafruit\_LiquidCrystal lcd\_1(0);



// Define pins

\#define phSensor A0      // Analog input for pH sensor

\#define piezoPin 2       // Buzzer for alerts



// System parameters (used for dosing calculation)

float systemVolumeL = 100.0;        

float base\_mL\_per\_pH\_per\_L = 4.0;   

float acid\_mL\_per\_pH\_per\_L = 3.5;   



// Used to avoid constantly rewriting same text to LCD

String lastRow1 = "";

String lastRow2 = "";



void setup()

{

&#x20; // Start LCD (16 columns, 2 rows)

&#x20; lcd\_1.begin(16, 2);

&#x20; lcd\_1.setBacklight(1);  // Turn on backlight



&#x20; // Set pin modes

&#x20; pinMode(phSensor, INPUT);

&#x20; pinMode(piezoPin, OUTPUT);

}



void loop()

{

&#x20; // Read raw sensor value (0–1023)

&#x20; int sensorValue = analogRead(phSensor);



&#x20; // Convert analog reading → pH scale (0–14)

&#x20; // left = acidic, right = basic

&#x20; float pHValue = 14.0 - (sensorValue / 1023.0) \* 14.0;



&#x20; // Keeps track of last time buzzer beeped

&#x20; static unsigned long lastBeep = 0;



&#x20; // Strings for LCD display

&#x20; String row1 = "";

&#x20; String row2 = "pH: " + String(pHValue, 2);



&#x20; // Check if pH is within safe range

&#x20; if (pHValue >= 6.6 \&\& pHValue <= 7.2)

&#x20; {

&#x20;   row1 = "Good!";        // System stable

&#x20;   noTone(piezoPin);      // No alarm needed

&#x20; }

&#x20; else if (pHValue < 6.6)

&#x20; {

&#x20;   // Too acidic → need to add base

&#x20;   float deviation = 6.6 - pHValue;



&#x20;   // Calculate required base amount (scaled to system volume)

&#x20;   float doseAmount = deviation \* systemVolumeL \* base\_mL\_per\_pH\_per\_L;



&#x20;   row1 = "Add Base:" + String((int)(doseAmount + 0.5)) + "mL";



&#x20;   // Beep once per second to alert user

&#x20;   if (millis() - lastBeep >= 1000)   // faster beep

&#x20;   {

&#x20;     tone(piezoPin, 1000);

&#x20;     delay(100);

&#x20;     noTone(piezoPin);

&#x20;     lastBeep = millis();

&#x20;   }

&#x20; }

&#x20; else

&#x20; {

&#x20;   // Too basic → need to add acid

&#x20;   float deviation = pHValue - 7.2;



&#x20;   // Calculate required acid amount

&#x20;   float doseAmount = deviation \* systemVolumeL \* acid\_mL\_per\_pH\_per\_L;



&#x20;   row1 = "Add Acid:" + String((int)(doseAmount + 0.5)) + "mL";



&#x20;   // Same alert logic

&#x20;   if (millis() - lastBeep >= 1000)   // faster beep

&#x20;   {

&#x20;     tone(piezoPin, 1000);

&#x20;     delay(100);

&#x20;     noTone(piezoPin);

&#x20;     lastBeep = millis();

&#x20;   }

&#x20; }



&#x20; // Only update LCD if text changed (prevents flickering)

&#x20; if (row1 != lastRow1)

&#x20; {

&#x20;   lcd\_1.setCursor(0, 0);

&#x20;   lcd\_1.print("                "); // Clear row

&#x20;   lcd\_1.setCursor(0, 0);

&#x20;   lcd\_1.print(row1);

&#x20;   lastRow1 = row1;

&#x20; }



&#x20; if (row2 != lastRow2)

&#x20; {

&#x20;   lcd\_1.setCursor(0, 1);

&#x20;   lcd\_1.print("                "); // Clear row

&#x20;   lcd\_1.setCursor(0, 1);

&#x20;   lcd\_1.print(row2);

&#x20;   lastRow2 = row2;

&#x20; }



&#x20; // Small delay for stability

&#x20; delay(50);

}



=====================================================

Makeshift/proof of concept Temperature sensor (prototype) CONTROL CODE

=====================================================



\#include <Adafruit\_LiquidCrystal.h>

Adafruit\_LiquidCrystal lcd\_1(0);



// Pin setup

const int tempPin = A0; // Connects to the TMP36 temperature sensor

const int buzzerPin = 9; // Connects to the buzzer output 



const float minTemp = 22.0; // Minimum temperature (°C) for fish to thrive in

const float maxTemp = 30.0; // Maximum temperature (°C) for fish to thrive in



// Setup 

void setup() {

&#x20; lcd\_1.begin(16,2); // LCD has 2 rows and 16 columns

&#x20; lcd\_1.setBacklight(1); // Turn on LCD backlight

&#x20; 

&#x20; // Labels on the LCD screen 

&#x20; lcd\_1.setCursor(0,1); 

&#x20; lcd\_1.print("Temp:"); 

&#x20; 

&#x20; // Set pin modes 

&#x20; pinMode(buzzerPin, OUTPUT); 

&#x20; pinMode(tempPin, INPUT); 

}



// Main loop

void loop() {

&#x20; // Read and smooth temperature sensor 

&#x20; int sensorValue = 0; 

&#x20; for (int i = 0; i < 10; i++) { // Take 10 readings for stability

&#x20;   sensorValue += analogRead(tempPin);

&#x20;   delay(5); // Short delay between readings

&#x20; }

&#x20; sensorValue /= 10; // Average the 10 readings 

&#x20; 

&#x20; // Convert analog reading to temperature 

&#x20; float voltage = sensorValue \* (5.0 / 1023.0); // Convert analog reading into voltage

&#x20; float temperatureC = (voltage - 0.5) \* 100.0; // TMP36 formula 

&#x20; temperatureC += 0.2; // Small calibration offset 

&#x20; temperatureC = constrain(temperatureC, -40.0, 125.0); // Limit temperature between -40°C and 125°C

&#x20; 

&#x20; lcd\_1.setCursor(0,0);

&#x20; lcd\_1.print("                "); // Clear row



&#x20; lcd\_1.setCursor(0,0);

&#x20; if (temperatureC < minTemp) {

&#x20;   lcd\_1.print("Too Chilly");

&#x20; }

&#x20; else if (temperatureC > maxTemp) {

&#x20;   lcd\_1.print("Too Warm");

&#x20; }

&#x20; else {

&#x20;   lcd\_1.print("Good");

&#x20; }

&#x20; 

&#x20; // Display temperature on LCD 

&#x20; lcd\_1.setCursor(6,1); // Position cursor at bottom row, column 6

&#x20; lcd\_1.print("   "); // Clear old numbers

&#x20; lcd\_1.setCursor(6,1);

&#x20; lcd\_1.print(round(temperatureC)); // Show temperature as whole number

&#x20; lcd\_1.print(" C "); // Add units of celsius

&#x20; 

&#x20; // Buzzer alerts 

&#x20; if (temperatureC < minTemp || temperatureC > maxTemp) {

&#x20;   tone(buzzerPin, 2000); // High tone for temp outside of range 

&#x20; }

&#x20; else {

&#x20;   noTone(buzzerPin); // No tone below threshold

&#x20; }

&#x20; 

&#x20; delay(200); // Small delay for smoother LCD updates

}



=====================================================

Makeshift/ proof of concept Water Level Sensor/valve opener CONTROL CODE

=====================================================

\#include <LiquidCrystal\_I2C.h>

\#include <Servo.h>

\#include <Wire.h>



// Initialize LCD and servo objects

LiquidCrystal\_I2C lcd(0x20,16,2);

Servo valveServo;



// Pin assignments

int echoPin = 6;       // Ultrasonic sensor echo pin

int triggerPin = 7;    // Ultrasonic sensor trigger pin

int servoPin = 9;      // Servo motor control pin



// Variables for distance calculation

long duration;

float distance;



// Threshold distance (if exceeded → water level is low)

float lowWaterThreshold = 30.0;



// Timing control for servo activation

unsigned long servoStart = 0;

unsigned long servoDuration = 8000; // Servo stays open for 8 seconds



// State tracking flags

bool servoOn = false;

bool activated = false;



// Servo and LCD Setup

void setup()

{

&#x20; Serial.begin(9600);  // Start serial for debugging



&#x20; // Set sensor pins

&#x20; pinMode(echoPin,INPUT);

&#x20; pinMode(triggerPin,OUTPUT);



&#x20; // Attach servo and set initial position (closed)

&#x20; valveServo.attach(servoPin);

&#x20; valveServo.write(0);



&#x20; // Initialize LCD

&#x20; lcd.init();

&#x20; lcd.backlight();

}



// Water Level Measurement

void loop()

{

&#x20; // Send ultrasonic pulse

&#x20; digitalWrite(triggerPin,LOW);

&#x20; delayMicroseconds(2);



&#x20; digitalWrite(triggerPin,HIGH);

&#x20; delayMicroseconds(10);



&#x20; digitalWrite(triggerPin,LOW);



&#x20; // Measure time for echo to return

&#x20; duration = pulseIn(echoPin,HIGH);



&#x20; // Convert time → distance (cm)

&#x20; distance = duration\*0.034/2;



&#x20; // Determine if water level is low (tank is too empty)

&#x20; bool lowWater = (distance > lowWaterThreshold);



&#x20; // If water is low and system hasn’t already activated

&#x20; if (lowWater \&\& !activated)

&#x20; {

&#x20;   valveServo.write(90);      // Open valve

&#x20;   servoStart = millis();     // Start timer

&#x20;   servoOn = true;

&#x20;   activated = true;          // Prevent repeated triggering

&#x20; }



&#x20; // After set time, close valve automatically

&#x20; if (servoOn \&\& millis() - servoStart >= servoDuration)

&#x20; {

&#x20;   valveServo.write(0);       // Close valve

&#x20;   servoOn = false;

&#x20; }



&#x20; // Reset activation flag once water level returns to normal

&#x20; if (!lowWater)

&#x20; {

&#x20;   activated = false;

&#x20; }



&#x20; // Display distance on LCD

&#x20; lcd.setCursor(0,0);

&#x20; lcd.print("Dist:");

&#x20; lcd.print(distance,1);

&#x20; lcd.print("cm   ");



&#x20; // Display system status

&#x20; lcd.setCursor(0,1);

&#x20; if (lowWater)

&#x20; {

&#x20;   lcd.print("Low water level");

&#x20; }

&#x20; else

&#x20; {

&#x20;   lcd.print("Level OK        ");

&#x20; }



&#x20; delay(200); // Small delay for stable readings

}



