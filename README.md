#include <LiquidCrystal.h>
#include<Servo.h>
#define trigPin 12
#define echoPin 9

Servo se;
Servo se1;
LiquidCrystal lcd(0, 1, 2, 3, 4, 5); /// REGISTER SELECT PIN,ENABLE PIN,D4 PIN,D5 PIN, D6 PIN, D7 PIN

int IR = 7; //IR at pin 7
int buz = 6; //buzzer at pin 6
int POWER=13;// led which tells the ON/OFF status
float duration = 0, distance = 0; //initializing duration and dustbin at 0
int flg = 0;
int openled=A0;
int closeled=A1;

void setup()
{
  pinMode(trigPin, OUTPUT);
  pinMode(buz, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(IR,INPUT);
  se.attach(10);
  se1.attach(11);
  pinMode(openled,OUTPUT);
  pinMode(closeled,OUTPUT);
  digitalWrite(IR,LOW);
  //digitalWrite(trigPin,HIGH);
  //digitalWrite(echoPin,LOW);
  pinMode(POWER,OUTPUT);
  digitalWrite(POWER,HIGH);
  digitalWrite(buz, LOW);
  se.write(25);
  se1.write(25);
  
  lcd.begin(16, 2);
  //lcd.setCursor(0,0);
  //lcd.print("Ready to Use");
}

void loop()
{
  delayMicroseconds(1000);
  
   if(digitalRead(7) == HIGH)
    {
      se.write(120);
      se1.write(120);
   
      analogWrite(openled,HIGH);
      analogWrite(closeled,LOW);
        digitalWrite(buz, HIGH);
      delayMicroseconds(2000);
      digitalWrite(buz,LOW);
      delayMicroseconds(1000);
      digitalWrite(buz,HIGH);
      delay(2000);
      digitalWrite(buz,LOW);
      analogWrite(openled,HIGH);
      analogWrite(closeled,LOW);
      
    
      flg =1;
    }
  
   else if(digitalRead(7) == LOW)
    {
      /*delay(3000);
      digitalWrite(buz, LOW);
      
      se.write(0);
      se1.write(0);*/

    //delayMicroseconds(300);
    analogWrite(openled,LOW);
    analogWrite(closeled,HIGH);
    while(flg)
    {
      delay(3000);
      digitalWrite(buz,HIGH);
      delay(1000);
      analogWrite(openled,LOW);
    analogWrite(closeled,HIGH);
      
      
      se.write(25);
      se1.write(25);
      digitalWrite(buz,LOW);
      digitalWrite(trigPin, LOW); 
      delayMicroseconds(2);
      digitalWrite(trigPin, HIGH);
      delayMicroseconds(10);
      digitalWrite(trigPin, LOW);

      duration = pulseIn(echoPin, HIGH);
      distance = (duration / 2) * 0.0344;
    
      flg=0;
    }

    //delayMicroseconds(500);   
    if(distance >=3 && distance <=5)
     {
       lcd.clear();
       lcd.print("Dustbin is Full");
       
     }

    else if(distance > 6)
      {
        lcd.clear();
        //lcd.setCursor(0,0);
        lcd.print("Distance = ");
        lcd.print(distance);
      }

     else
     {
        lcd.setCursor(0,0);
        lcd.print("Ready To Use");
        
     }
     
   }
}
  

