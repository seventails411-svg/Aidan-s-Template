#include <IRremote.h>
#include <SoftwareSerial.h>

#define RX_PIN 3
#define TX_PIN 4


SoftwareSerial mySerial(RX_PIN, TX_PIN);


const int IR_RECEIVE_PIN = 12;  // Define the pin number for the IR Sensor

const int A_1B = 5;
const int A_1A = 6;
const int B_1B = 9;
const int B_1A = 10;


int speed = 150;

void setup() {
 
  pinMode(LED_BUILTIN, OUTPUT);




  Serial.begin(9600);
  mySerial.begin(9600);
  //motor
  pinMode(A_1B, OUTPUT);
  pinMode(A_1A, OUTPUT);
  pinMode(B_1B, OUTPUT);
  pinMode(B_1A, OUTPUT);

  //IR remote
  IrReceiver.begin(IR_RECEIVE_PIN, ENABLE_LED_FEEDBACK);  // Start the IR receiver // Start the receiver
  Serial.println("REMOTE CONTROL START");

}

void loop() {
digitalWrite(LED_BUILTIN,LOW);
  if (mySerial.available()>0) {
    //    Serial.println(results.value,HEX);
    char key = mySerial.read();
    if (key != "ERROR") {
      Serial.println(key);
  
      if (key == '+') {
        speed += 50;
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '-') {
        speed -= 50;
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '2') {
        moveForward(speed);
        delay(1000);
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '1') {
        moveLeft(speed);
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '3') {
        moveRight(speed);
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '4') {
        turnLeft(speed);
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '6') {
        turnRight(speed);
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '7') {
        backLeft(speed);
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '9') {
        backRight(speed);
        Serial.println("speed =");
        Serial.println(speed);
      } else if (key == '8') {
        moveBackward(speed);
        delay(1000);
        Serial.println("speed =");
        Serial.println(speed);
      }

      if (speed >= 255) {
        speed = 255;
      }
      if (speed <= 0) {
        speed = 0;
      }
      delay(500);
      stopMove();
    }

    IrReceiver.resume();  // Enable receiving of the next value
  }
}

void moveForward(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void moveBackward(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void turnRight(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void turnLeft(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void moveLeft(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}

void moveRight(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);
}

void backLeft(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}

void backRight(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);
}

void stopMove() {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);
}


String decodeKeyValue(long result)
{
  switch(result){
    case 0x16:
      return "0";
    case 0xC:
      return "1"; 
    case 0x18:
      return "2"; 
    case 0x5E:
      return "3"; 
    case 0x8:
      return "4"; 
    case 0x1C:
      return "5"; 
    case 0x5A:
      return "6"; 
    case 0x42:
      return "7"; 
    case 0x52:
      return "8"; 
    case 0x4A:
      return "9"; 
    case 0x9:
      return "+"; 
    case 0x15:
      return "-"; 
    case 0x7:
      return "EQ"; 
    case 0xD:
      return "U/SD";
    case 0x19:
      return "CYCLE";         
    case 0x44:
      return "PLAY/PAUSE";   
    case 0x43:
      return "FORWARD";   
    case 0x40:
      return "BACKWARD";   
    case 0x45:
      return "POWER";   
    case 0x47:
      return "MUTE";   
    case 0x46:
      return "MODE";       
    case 0x0:
      return "ERROR";   
    default :
      return "ERROR";
    }
}

