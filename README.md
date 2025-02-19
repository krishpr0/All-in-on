# All-in-on
```
// Pin Definitions
int soilMoisturePin = A2;  // Soil sensor input
int soilPump = 5;          // Relay for Water Pump (Irrigation)

int ldrPin = A1;           // LDR sensor input
int lightOutput = 9;       // Transistor base for LED (Night Light)

int trigPin = 6;           // Ultrasonic Trigger
int echoPin = 7;           // Ultrasonic Echo
int tapMotor = 8;          // Relay for Water Tap Motor

void setup() {
    Serial.begin(9600);

    pinMode(soilPump, OUTPUT);
    pinMode(lightOutput, OUTPUT);
    pinMode(tapMotor, OUTPUT);
    pinMode(trigPin, OUTPUT);
    pinMode(echoPin, INPUT);

    digitalWrite(soilPump, HIGH);   // Default OFF (Relay Active LOW)
    digitalWrite(lightOutput, HIGH);// Default OFF (PNP Transistor)
    digitalWrite(tapMotor, HIGH);   // Default OFF (Relay Active LOW)
}

void loop() {
    // 🌱 Soil Moisture Sensor Logic
    int soilMoisture = analogRead(soilMoisturePin);
    Serial.print("Soil Moisture: ");
    Serial.println(soilMoisture);
    
    if (soilMoisture > 700) { // Dry Soil (Adjust Threshold)
        digitalWrite(soilPump, LOW); // Turn ON Pump
    } else {
        digitalWrite(soilPump, HIGH); // Turn OFF Pump
    }

    // 💡 LDR Sensor Logic for Night Light
    int lightLevel = analogRead(ldrPin);
    Serial.print("Light Level: ");
    Serial.println(lightLevel);

    if (lightLevel < 700) { // Darkness Detected
        digitalWrite(lightOutput, LOW); // Turn ON Light
    } else {
        digitalWrite(lightOutput, HIGH); // Turn OFF Light
    }

    // 🚰 Ultrasonic Sensor for Automatic Tap
    long duration;
    int distance;
    
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    
    duration = pulseIn(echoPin, HIGH);
    distance = duration * 0.034 / 2; // Convert to cm

    Serial.print("Distance: ");
    Serial.println(distance);

    if (distance < 20) { // If person is near the tap
        digitalWrite(tapMotor, LOW); // Turn ON Water Motor
    } else {
        digitalWrite(tapMotor, HIGH); // Turn OFF Water Motor
    }

    delay(500);
}
