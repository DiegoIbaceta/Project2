#include <Servo.h>
#include <LiquidCrystal.h>
const int trigPin = 11;
const int echoPin = 12;
#define sensorOnOff 7
#define sensorPin A0
#define sensorLuzPin A5
#define servoPin1 9
#define servoPin2 8
#define piezoPin 10
Servo miServo;
Servo miServo2;
int humedadDeseada = 500;
int anguloAbierto = 180;
int anguloCerrado = 0;
int luzdeceada = 535;
int anguloAbierto2 = 180;
int anguloCerrado2 = 0;

void setup() {
  pinMode(sensorOnOff, OUTPUT);
  digitalWrite(sensorOnOff, LOW);
  Serial.begin(9600);
  miServo.attach(servoPin1);
  miServo2.attach(servoPin2);
  pinMode(piezoPin, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.println("Configuración lista");
}

void loop() {
  int distancia = getDistance();
  Serial.print("Distancia en cm: ");
  Serial.print(distancia);
  Serial.println(" cm");
  delay(1000);
  digitalWrite(sensorOnOff, HIGH);
  delay(10);
  int valor = analogRead(sensorPin);
  Serial.print("Nivel de humedad: ");
  Serial.println(valor);
  int valorLuz = analogRead(sensorLuzPin);
  Serial.print("Nivel de luz: ");
  Serial.println(valorLuz);
  if (distancia < 150) {
    digitalWrite(piezoPin, HIGH);
  } else {
    digitalWrite(piezoPin, LOW);
  }
  if (valor > humedadDeseada) {
    miServo.write(anguloAbierto);
  } else {
    miServo.write(anguloCerrado);
  }
  if (valorLuz > luzdeceada) {
    miServo2.write(anguloAbierto2);
  } else {
    miServo2.write(anguloCerrado2);
  }
  digitalWrite(sensorOnOff, LOW);
  delay(1000);
}
int getDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  long duration = pulseIn(echoPin, HIGH);
  int cm = duration / 58;
  return cm;
}