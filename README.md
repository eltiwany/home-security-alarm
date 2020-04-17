# home-security-alarm
Home Security Alarm System Using Arduino and Ultrasonic Sensor HC-SR04

# Configuration
1. Ultrasonic Sensor
    VCC - 5V
    Trig - D10
    Echo - D9
    GND - GND
    
2. Red LED    - D8
   Green LED  - D7

3. Buzzer
    VCC - D2
    GND - GND

4. Reset Button (Push Button)
    Terminal I - D11
    Terminal II - GND

# Arduino Code
```
// defines pins numbers
const int trigPin = 10;
const int echoPin = 9;
const int redLed = 8;
const int greenLed = 7;
const int buzzer = 2;
const int resetBtn = 11;

void alarmOn() {
  digitalWrite(greenLed, LOW);
  digitalWrite(redLed, HIGH);
  digitalWrite(buzzer, HIGH);
  delay(300);
  digitalWrite(redLed, LOW);
  digitalWrite(buzzer, LOW);
  delay(300);
}

void alarmOff() {
  digitalWrite(redLed, LOW);
  digitalWrite(buzzer, LOW);
  digitalWrite(greenLed, HIGH);
  delay(600);
}

void alarmDisabled() {
  digitalWrite(redLed, HIGH);
  digitalWrite(buzzer, LOW);
  digitalWrite(greenLed, HIGH);
  delay(600);
}

// defines variables
long duration;
int distance;
int alarmTrig = 0;
int alarmActive = 0;

void setup() {
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin, INPUT); // Sets the echoPin as an Input
  pinMode(resetBtn, INPUT);
  digitalWrite(resetBtn, HIGH);
  alarmOff();
  Serial.begin(9600); // Starts the serial communication
}
void loop() {
  
  if(digitalRead(resetBtn) == LOW && alarmActive == 0) {
    alarmActive = 1;
    delay(500);
  }

  if(digitalRead(resetBtn) == LOW && alarmActive == 1) {
    alarmActive = 0;
    alarmTrig = 0;
    alarmDisabled();
  }

  if(alarmActive == 1) {
    // Clears the trigPin
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    // Sets the trigPin on HIGH state for 10 micro seconds
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
    // Reads the echoPin, returns the sound wave travel time in microseconds
    duration = pulseIn(echoPin, HIGH);
    // Calculating the distance
    distance= duration*0.034/2;
    // Prints the distance on the Serial Monitor
    Serial.print("Distance: ");
    Serial.println(distance);
    
    if (distance <= 25 || alarmTrig == 1) {
      alarmOn();
      alarmTrig = 1;
    }else{
      alarmOff();
    }
  }
}
```
    
