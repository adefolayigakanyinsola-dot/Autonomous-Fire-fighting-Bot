#🔥 Autonomous-Fire-fighting-Bot


**Autonomous Firefighting Robot** — An Arduino-based robot using IR flame sensors to detect fire direction, navigate toward it via motorized wheels, then extinguish it with a servo-aimed water pump. Includes buzzer/LED alerts and dual power rails for stable sensor readings. A low-cost, autonomous fire-response solution for small enclosed spaces.



##☑️: Overview
This project is an autonomous mobile robot designed to detect and respond to small open flames in an environment. It uses three IR flame sensors positioned front, left, and right to determine the direction of a fire, drives its wheels toward the detected flame, and activates a water pump — aimed by a micro servo — to extinguish it. A buzzer and LED indicators provide real-time status feedback (Standby / Detecting / Extinguishing).





##🤖 Features

-💥 Multi-directional flame detection (3 IR flame sensors)

-🚀 Autonomous navigation toward detected fire

-✳️ Servo-aimed water nozzle

-🕐 Audible buzzer alert

-🧠 Visual LED status indicators (Green/Yellow/Red)

-🌙 Manual push-button 



[Component_Worksheet.xlsx](https://github.com/user-attachments/files/30321609/Component_Worksheet.xlsx)







#define enA 10  //Enable1 L298 Pin enA
#define in1 9   //Motor1  L298 Pin in1
#define in2 8   //Motor1  L298 Pin in2
#define in3 7   //Motor2  L298 Pin in3
#define in4 6   //Motor2  L298 Pin in4
#define enB 5   //Enable2 L298 Pin enB
#define ir_R A0 
#define ir_F A1 //vvvvvvvvvvvv
#define ir_L A2
#define servo A4
#define pump A5

int Speed = 160; // Write The Duty Cycle 0 to 255 Enable for Motor Speed
int s1, s2, s3;

void servoPulse(int pin, int angle) {
  int pwm = (angle * 11) + 500; // Convert angle to microseconds
  digitalWrite(pin, HIGH);
  delayMicroseconds(pwm);
  digitalWrite(pin, LOW);
  delay(50); // Refresh cycle of servo
}

void forword() { //forword
  digitalWrite(in1, HIGH); //Right Motor forword Pin
  digitalWrite(in2, LOW);  //Right Motor backword Pin
  digitalWrite(in3, LOW);  //Left Motor backword Pin
  digitalWrite(in4, HIGH); //Left Motor forword Pin
}

void backword() { //backword
  digitalWrite(in1, LOW);  //Right Motor forword Pin
  digitalWrite(in2, HIGH); //Right Motor backword Pin
  digitalWrite(in3, HIGH); //Left Motor backword Pin
  digitalWrite(in4, LOW);  //Left Motor forword Pin
}

void turnRight() { //turnRight
  digitalWrite(in1, LOW);  //Right Motor forword Pin
  digitalWrite(in2, HIGH); //Right Motor backword Pin
  digitalWrite(in3, LOW);  //Left Motor backword Pin
  digitalWrite(in4, HIGH); //Left Motor forword Pin
}

void turnLeft() { //turnLeft
  digitalWrite(in1, HIGH); //Right Motor forword Pin
  digitalWrite(in2, LOW);  //Right Motor backword Pin
  digitalWrite(in3, HIGH); //Left Motor backword Pin
  digitalWrite(in4, LOW);  //Left Motor forword Pin
}

void Stop() { //stop
  digitalWrite(in1, LOW); //Right Motor forword Pin
  digitalWrite(in2, LOW); //Right Motor backword Pin
  digitalWrite(in3, LOW); //Left Motor backword Pin
  digitalWrite(in4, LOW); //Left Motor forword Pin
}

void setup() { // put your setup code here, to run once
  Serial.begin(9600); // start serial communication at 9600bps
  // pinMode(ir_R, INPUT);   // declare fire sensor pin as input
  pinMode(ir_F, INPUT);   // declare fire sensor pin as input
  // pinMode(ir_L, INPUT);   // declare fire sensor pin as input
  pinMode(enA, OUTPUT);   // declare as output for L298 Pin enA
  pinMode(in1, OUTPUT);   // declare as output for L298 Pin in1
  pinMode(in2, OUTPUT);   // declare as output for L298 Pin in2
  pinMode(in3, OUTPUT);   // declare as output for L298 Pin in3
  pinMode(in4, OUTPUT);   // declare as output for L298 Pin in4
  pinMode(enB, OUTPUT);   // declare as output for L298 Pin enB
  pinMode(servo, OUTPUT);
  pinMode(pump, OUTPUT);
  Serial.println("Initializing Motors....");

  for (int angle = 90; angle <= 140; angle += 5) {
    servoPulse(servo, angle);
  }
  for (int angle = 140; angle >= 40; angle -= 5) {
    servoPulse(servo, angle);
  }
  for (int angle = 40; angle <= 95; angle += 5) {
    servoPulse(servo, angle);
  }

  analogWrite(enA, Speed); // Write The Duty Cycle 0 to 255 Enable Pin A for Motor1 Speed
  analogWrite(enB, Speed); // Write The Duty Cycle 0 to 255 Enable Pin B for Motor2 Speed
  delay(500);
}

void loop() {
  // s1 = analogRead(ir_R);
  s2 = analogRead(ir_F);
  // s3 = analogRead(ir_L);

  //=============================================================
  // Auto Control
  //=============================================================
  Serial.print("Flame:\t");
  Serial.println(s2);
  // Serial.print(s2);
  // Serial.print("\t");
  // Serial.println(s3);
  delay(50);

  if (s2 < 350) {
    Stop();
    digitalWrite(pump, 1);
    for (int angle = 90; angle <= 140; angle += 3) {
      servoPulse(servo, angle);
    }
    for (int angle = 140; angle >= 40; angle -= 3) {
      servoPulse(servo, angle);
    }
    for (int angle = 40; angle <= 90; angle += 3) {
      servoPulse(servo, angle);
    }
  }
  else if (s2 >= 251 && s2 <= 800) {
    digitalWrite(pump, 0);
    backword();
    delay(100);
    turnRight();
    delay(200);
  }
  else {
    digitalWrite(pump, 0);
    backword();
    delay(100);
    turnRight();
    delay(200);
    turnRight();
    delay(200);
  }

  delay(10);
}








