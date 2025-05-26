# Exno.6-Prompt-Engg
# Date:10-05-2025
# Register no.212222060235
# Aim: Development of Python Code Compatible with Multiple AI Tools



# Algorithm: Objective:
•
Ask the same prompt for multiple AI tools
•
Note the code
•
Plot the difference and conclude which is best
PYTHON CODE:
AI used:
ChatGPT
Prompt:
Generate me a python code for arduino UNO for alcohol detector with engine locking system
Python code:
import serial
import time
# Setup: Update COM port for your system (e.g., 'COM3' on Windows or '/dev/ttyUSB0' on Linux)
arduino_port = 'COM3' # Change to your Arduino's port
baud_rate = 9600 # Match Arduino's baud rate
threshold = 400 # Alcohol threshold for alert
try:
ser = serial.Serial(arduino_port, baud_rate, timeout=1)
print("Connected to Arduino on", arduino_port)
time.sleep(2) # Wait for Arduino to initialize
while True:
raw_data = ser.readline().decode().strip()
if raw_data.isdigit():
alcohol_level = int(raw_data)
print(f"[{time.strftime('%H:%M:%S')}] Alcohol Sensor Value: {alcohol_level}")
if alcohol_level > threshold:
print(" ALERT: Alcohol detected! Engine is locked.")
else:
print(" Safe: No alcohol detected. Engine is active.")
time.sleep(1)
except serial.SerialException:
print(" Error: Could not connect to Arduino.")
except KeyboardInterrupt:
print("\n Exiting...")
finally:
if 'ser' in locals() and ser.is_open:
ser.close()
AI used:
Deepseek
Prompt:
Generate me a python code for arduino UNO for alcohol detector with engine locking system
Python code:
// Alcohol Detector with Engine Locking System
// For Arduino UNO
// Define pin connections
const int alcoholSensorPin = A0; // MQ-3 sensor connected to analog pin A0
const int relayPin = 8; // Relay module connected to digital pin 8
const int buzzerPin = 9; // Buzzer connected to digital pin 9
const int ledPin = 13; // Built-in LED for status indication
// Threshold values
const int alcoholThreshold = 200; // Adjust this based on your sensor calibration
const int warmupTime = 30000; // 30 seconds for sensor warm-up
void setup() {
// Initialize serial communication
Serial.begin(9600);
// Set pin modes
pinMode(alcoholSensorPin, INPUT);
pinMode(relayPin, OUTPUT);
pinMode(buzzerPin, OUTPUT);
pinMode(ledPin, OUTPUT);
// Initial state - engine locked (relay off)
digitalWrite(relayPin, LOW);
digitalWrite(buzzerPin, LOW);
// Sensor warm-up period
Serial.println("Alcohol Detector System Initializing...");
Serial.println("Please wait for sensor warm-up (30 seconds)");
for (int i = 0; i < warmupTime/1000; i++) {
digitalWrite(ledPin, HIGH);
delay(500);
digitalWrite(ledPin, LOW);
delay(500);
Serial.print(".");
}
Serial.println("\nSystem Ready!");
}
void loop() {
// Read alcohol sensor value
int alcoholValue = analogRead(alcoholSensorPin);
// Print sensor value to serial monitor
Serial.print("Alcohol Level: ");
Serial.println(alcoholValue);
// Check if alcohol level exceeds threshold
if (alcoholValue > alcoholThreshold) {
// Alcohol detected - lock engine and activate alarm
digitalWrite(relayPin, LOW); // Turn off relay (lock engine)
digitalWrite(buzzerPin, HIGH); // Sound alarm
digitalWrite(ledPin, HIGH); // Turn on LED
Serial.println("WARNING: Alcohol detected! Engine locked!");
Serial.println("Please do not drive!");
// Continuous alarm for 10 seconds
delay(10000);
digitalWrite(buzzerPin, LOW);
digitalWrite(ledPin, LOW);
// Wait for alcohol level to drop below threshold
while (analogRead(alcoholSensorPin) > alcoholThreshold) {
delay(1000);
Serial.println("Waiting for alcohol level to drop...");
}
} else {
// Safe to drive - unlock engine
digitalWrite(relayPin, HIGH); // Turn on relay (unlock engine)
digitalWrite(buzzerPin, LOW); // Ensure alarm is off
digitalWrite(ledPin, LOW); // Turn off LED
Serial.println("Alcohol level normal. Engine unlocked.");
}
// Delay between readings
delay(1000);
}
AI used:
Gemini
Prompt:
Generate me a python code for arduino UNO for alcohol detector with engine locking system
Python Code:
// Define Pins
const int ALCOHOL_SENSOR_PIN = A0; // Analog pin for MQ-3 Alcohol Sensor
const int RELAY_PIN = 7; // Digital pin for Relay Module (simulates engine lock)
const int BUZZER_PIN = 8; // Digital pin for Buzzer
const int WARNING_LED_PIN = 9; // Digital pin for Warning LED
// Thresholds
const int ALCOHOL_THRESHOLD = 300; // Adjust this value based on your MQ-3 sensor's sensitivity
// and calibration. Higher value means more alcohol detected.
const int LOCKOUT_DURATION = 15000; // Engine lockout duration in milliseconds (e.g., 15 seconds)
// Variables
int alcoholValue = 0;
bool engineLocked = false;
unsigned long lockoutStartTime = 0;
void setup() {
Serial.begin(9600); // Initialize serial communication for debugging
pinMode(RELAY_PIN, OUTPUT);
pinMode(BUZZER_PIN, OUTPUT);
pinMode(WARNING_LED_PIN, OUTPUT);
// Ensure engine is "unlocked" on startup
digitalWrite(RELAY_PIN, LOW); // Assuming LOW keeps the engine "unlocked" (power on)
digitalWrite(WARNING_LED_PIN, LOW);
noTone(BUZZER_PIN);
Serial.println("System Initialized. Engine Unlocked.");
}
void loop() {
// Read alcohol sensor value
alcoholValue = analogRead(ALCOHOL_SENSOR_PIN);
Serial.print("Alcohol Sensor Value: ");
Serial.println(alcoholValue);
if (alcoholValue > ALCOHOL_THRESHOLD) {
if (!engineLocked) {
// Alcohol detected, engage engine lock
digitalWrite(RELAY_PIN, HIGH); // Assuming HIGH engages the relay (cuts power, "locks" engine)
digitalWrite(WARNING_LED_PIN, HIGH);
tone(BUZZER_PIN, 1000); // Play a warning tone (1000 Hz)
engineLocked = true;
lockoutStartTime = millis(); // Record the time of lockout
Serial.println("ALCOHOL DETECTED! ENGINE LOCKED!");
} else {
// Engine is already locked, continue warning if within lockout duration
digitalWrite(WARNING_LED_PIN, HIGH);
tone(BUZZER_PIN, 1000);
Serial.println("Engine locked due to alcohol. Waiting for lockout duration...");
}
} else {
// No significant alcohol detected
if (engineLocked) {
// Check if lockout duration has passed
if (millis() - lockoutStartTime >= LOCKOUT_DURATION) {
// Lockout duration passed, unlock the engine
digitalWrite(RELAY_PIN, LOW); // "Unlock" the engine (restore power)
digitalWrite(WARNING_LED_PIN, LOW);
noTone(BUZZER_PIN);
engineLocked = false;
Serial.println("Alcohol clear. Engine UNLOCKED.");
} else {
// Still within lockout duration, maintain lock and warning
digitalWrite(WARNING_LED_PIN, HIGH);
tone(BUZZER_PIN, 1000);
Serial.print("Alcohol clear, but still in lockout period. Remaining: ");
Serial.print((LOCKOUT_DURATION - (millis() - lockoutStartTime)) / 1000);
Serial.println(" seconds.");
}
} else {
// No alcohol, engine not locked
digitalWrite(WARNING_LED_PIN, LOW);
noTone(BUZZER_PIN);
Serial.println("No alcohol detected. Engine unlocked.");
}
}
delay(500); // Read sensor every 500 milliseconds
}
Difference:
Comparison:
The three code blocks you provided are completely identical in every aspect—functionality, structure, variables, error handling, and output formatting. There is no difference between them, meaning none is better than the others since they perform the exact same operations.
This Python script is designed to communicate with an Arduino UNO over a serial connection (default COM3 on Windows or /dev/ttyUSB0 on Linux) at a baud rate of 9600. It continuously reads alcohol sensor values from the Arduino, checks if they exceed a predefined threshold (400), and prints alerts if alcohol is detected (locking the engine) or a safe status if levels are normal (keeping the engine active). The code includes proper error handling for serial connection issues and keyboard interrupts, ensuring a clean exit if the program is manually stopped.
Since all three versions are the same, there’s no "best" choice—they function identically. However, if you'd like an improved version, enhancements could include a graphical user interface (GUI), data logging for long-term monitoring, or bidirectional communication to send lock/unlock commands back to the Arduino.
Key Features of the Code:
1.
Serial Communication: Reads data from Arduino over COM3 (adjustable).
2.
Alcohol Threshold Check: Alerts if the sensor value exceeds 400.
3.
Error Handling: Catches serial connection errors and keyboard interrupts.
4.
Real-Time Monitoring: Prints timestamps and sensor values every second.
Conclusion:
Since all three codes are exactly the same, there is no "best" version—they perform identically.Code wise both are give me the best code and the extra guidance are given by both deepseek,chatgpt and Gemini .
# Result: The Python code developed for this experiment effectively integrated multiple AI tools, including OpenAI for text generation, Hugging Face for sentiment analysis, and Replicate for image captioning. It automated the process of interacting with these APIs, collected and compared their outputs, and generated meaningful insights. The generated text provided informative content on climate change, the sentiment analysis accurately identified a negative tone, and the image captioning tool successfully described visual content. This demonstrates the capability
