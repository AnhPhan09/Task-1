const int trigPin = 4;
const int echoPin = 5;
const int led = 6;
const int butt = 8;
const int buzz = 13;
const int pot = A1;
const int sound = A3;
const int In1 = 9;
const int In2 = 10;
const int EnA = 3; 
#include <Servo.h>
Servo myservo;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(led, OUTPUT);
  pinMode(butt, INPUT);
  pinMode(buzz, OUTPUT);
  pinMode(pot, INPUT);
  pinMode(sound, INPUT);
  pinMode(In1, OUTPUT);
  pinMode(In2, OUTPUT);
  pinMode(EnA, OUTPUT); 
  myservo.attach(7);
  Serial.begin(9600);
}

void loop()
{
  buzzingbutton();
  ultraled();
  potentiometerServo();
  soundmotor();
}
void ultraled()
{
  long duration, cm;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(5);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  cm = microsecondsToCentimetres(duration);
  if (cm < 10)  
  {
    digitalWrite(led, HIGH);
  }
  else
  {
    digitalWrite(led, LOW);
  }
}
void buzzingbutton()
{
  if(digitalRead(butt) == HIGH)
  {
    Serial.println("butt");
    tone(buzz, 1000);
  }
  else
  {
    noTone(buzz);
  }
}
void potentiometerServo()
{
  int input = analogRead(pot);
  //Serial.println(input/7);
  myservo.write(input/7);
}  
void soundmotor()
{
  Serial.println(analogRead(sound));
  int idk = analogRead(sound);
  if (idk > 50)
  {
    digitalWrite(In1, HIGH);
    digitalWrite(In2, LOW);
    analogWrite(EnA, 200);
  }
  else
  {
    digitalWrite(In1, LOW);
    digitalWrite(In2, LOW); 
  } 
}
long microsecondsToCentimetres(long microseconds)
{
  return microseconds / 29 / 2;
}
