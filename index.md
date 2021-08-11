# Obstacle Avoiding Robot
I am student learning at BlueStamp Engineering and I am working on an Obstacle Avoiding Robot with Arduino. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Brendon V | Bard High School Early College Manhattan | Computer Engineering | Graduated

![Headstone Image]
  
# Final Milestone

[![Final Milestone]{:target="_blank" rel="noopener"}

# Second Milestone
My second milestone is the increased accuracy of my robot. After my first milestone. I had put tape on the wheels because the pressure on the motors were too much for them to handle and that caused the robot to be very slow. Puttin tape on the wheels proved very efficient in increased speed and reliability. As for the sensor, I had recoded the distance for the waves that emanate for other surfaces so that the robot no longer comes too close into contact with something. 

[![Third Milestone]{:target="_blank" rel="noopener"}
# First Milestone
  

My first milestone was being able to have the robot perform all the directions. Go back, go forwawrd, go left and go right, pretty straightforward. First of what I had to do was build the chassis for where all the components will be mounted and as for the motors to help move the robot. Then I wired and sodered the motors to the motor driver. This was tough since some of the output screws weren't budging so I had to get a new one. After wiring the motors to the driver, I wired the driver to the arduino so that I can outsource the code into it and run it. Before all of that, I needed a power source for the motor driver. So I went and used 4 double A batteries to power the driver. Not only that, I did some research on how to wire the micro servo and the ultrasonic sensors to the arduino. As a result, I wired those together and began with coding. Now my robot was able to make the turns that I set out for it to do. 

[![First Milestone](https://www.youtube.com/watch?v=bE2VUoPaWz0){:target="_blank" rel="noopener"}

# Circuit Diagram
![Screenshot (64)](https://user-images.githubusercontent.com/20545400/129069589-e4d4bf8c-44c0-4676-857c-d9b5c2e92ea9.png)
# Code 
```C++

#include <Servo.h>
#include <NewPing.h>

int motor1pin1 = 9;
int motor1pin2 = 12;
int motor2pin1 = 8;
int motor2pin2 = 13;
int ENA = 3;
int ENB = 11;

#define trig_pin 7 //analog input 1
#define echo_pin A2 //analog input 2

#define maximum_distance 200

int distance = 100;
boolean goesForward = false;

NewPing sonar(trig_pin, echo_pin, maximum_distance); //sensor function
Servo servo_motor;

int readPing(){
  delay(70);
  int cm = sonar.ping_cm();
  if (cm==0){
    cm=250;
  }
  return cm;
}

int lookRight(){ 
  servo_motor.write(10);
  delay(500);
  int distance = readPing();
  delay(100);
  servo_motor.write(90);
  return distance;
}

int lookLeft(){
  servo_motor.write(170);
  delay(500);
  int distance = readPing();
  delay(100);
  servo_motor.write(90);
  return distance;
  delay(100);
}

void moveStop(){
 
  digitalWrite(motor1pin2, LOW);
  digitalWrite(motor2pin2, LOW);
  digitalWrite(motor1pin1, LOW);
  digitalWrite(motor2pin1, LOW);
}

void moveForward(){

  if(!goesForward){

    goesForward=true;
   
    digitalWrite(motor2pin2, HIGH);
    digitalWrite(motor1pin2, HIGH);
 
    digitalWrite(motor2pin1, LOW);
    digitalWrite(motor1pin1, LOW);
  }
}

void moveBackward(){

  goesForward=false;

  digitalWrite(motor2pin1, HIGH);
  digitalWrite(motor1pin1, HIGH);
 
  digitalWrite(motor2pin2, LOW);
  digitalWrite(motor1pin2, LOW);
 
}

void turnRight(){

  digitalWrite(motor2pin2, HIGH);
  digitalWrite(motor1pin1, HIGH);
  analogWrite(ENA, 195);
  digitalWrite(motor2pin1, LOW);
  digitalWrite(motor1pin2, LOW);
  analogWrite(ENB, 195);
  delay(500);
 
  digitalWrite(motor2pin2, HIGH);
  digitalWrite(motor1pin2, HIGH);
  analogWrite(ENA, 195);
  digitalWrite(motor2pin1, LOW);
  digitalWrite(motor1pin1, LOW);

}

void turnLeft(){

  digitalWrite(motor2pin1, HIGH);
  digitalWrite(motor1pin2, HIGH);
  analogWrite(ENA, 195);
  digitalWrite(motor2pin2, LOW);
  digitalWrite(motor1pin1, LOW);
  analogWrite(ENB, 195);
  delay(500);
 
  digitalWrite(motor2pin2, HIGH);
  digitalWrite(motor1pin2, HIGH);
  analogWrite(ENA, 195);
  digitalWrite(motor2pin1, LOW);
  digitalWrite(motor1pin1, LOW);
  analogWrite(ENB, 195);
}

void setup() {
    // put your setup code here, to run once:
    pinMode(motor1pin1, OUTPUT);
    pinMode(motor1pin2, OUTPUT);
    pinMode(motor2pin1, OUTPUT);
    pinMode(motor2pin2, OUTPUT);
    pinMode(ENA, OUTPUT);
    pinMode(ENB, OUTPUT);
    Serial.begin(9600);
   
    pinMode(trig_pin, OUTPUT);
    pinMode(echo_pin, INPUT);
   
    servo_motor.attach(10); //our servo pin

  servo_motor.write(90);
  delay(2000);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
  distance = readPing();
  delay(100);
   
}

void loop() { // put your main code here, to run repeatedly:
    int distanceRight = 0;
    int distanceLeft = 0;
    delay(50);

    if (distance <= 20){
       moveStop();
       delay(300);
       moveBackward();
       delay(400);
       moveStop();
       delay(300);
       distanceRight = lookRight();
       delay(300);
       distanceLeft = lookLeft();
       delay(300);
   
    if (distance >= distanceLeft){
       turnRight();
       moveStop();
    }
    else{
      turnLeft();
      moveStop();
    }
   }
   else{
     moveForward();
   }
     distance = readPing();
}
```

