# -
 LCD显示；智能控制；定时器控制；实时监控#include<SoftwareSerial.h>
#include<Wire.h>
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27,16,2);
SoftwareSerial mySerial(12,13);
int sum1=0,sum2=0,sum3=0,sum4=0;
int  ir=A0;//定义光敏电阻的pwm引脚为A0脚;
int YU_light;
int  RED_ir1=22;//定义红外避障引脚;
int  RED_ir2=24;
int  RED_ir3=26;
int  RED_ir4=28;
bool RED1;//定义布尔量存储
bool RED2;
bool RED3;
bool RED4;
int trig1=11;//触点1
int trig2=10;//
int trig3=9;//
int trig4=8;
void setup() {
   lcd.begin();
   Serial.begin(9600);//打开串口序列埠
   mySerial.begin(9600);
   pinMode(ir,INPUT);
   for(int i=2;i<6;i++){
   pinMode(i,OUTPUT);
   }
   pinMode(RED_ir1,INPUT);
   pinMode(RED_ir2,INPUT);
   pinMode(RED_ir3,INPUT);
   pinMode(RED_ir4,INPUT);
   pinMode(trig1,INPUT);
   pinMode(trig2,INPUT);
   pinMode(trig3,INPUT);
   pinMode(trig4,INPUT);
   
}

void loop() {
  RED1=digitalRead(RED_ir1);//读取红外避障到bool量;
  RED2=digitalRead(RED_ir2);
  RED3=digitalRead(RED_ir3);
  RED4=digitalRead(RED_ir4);
   Serial.println(RED1);//输出bool量到串口序列埠中;
   Serial.println(RED2);
   Serial.println(RED3);
   Serial.println(RED4);
   YU_light= analogRead(A0);//读取光敏电阻的pwm值到串口序列埠中(0~1023);
int LED_light=  map(YU_light,0,1023,200,0);

int contact =map(YU_light,0,1023,200,255);//给一个触点信号：
   Serial.print("YU_light==");
   Serial.println(YU_light);
   Serial.print("contact==");
   Serial.println(contact);
   Serial.print("LED_light==");
   Serial.println(LED_light);
   lcd.cursor();
   
if ( RED1 == 0){
     if(YU_light<=700){
      analogWrite(trig1,contact);
      analogWrite(2,LED_light);
    }
   else {
    analogWrite(trig1,LOW);
   }
     lcd.setCursor(0,0);
     lcd.print("A");
       sum1+=1;
       delay(1000);
      lcd.setCursor(0,1);
      lcd.print(sum1,DEC);
  byte packet1[2];
  packet1[0]= 97;
  packet1[1]=sum1;
  Serial.println(sum1);
    if(mySerial.available() > 0){//check BT is succeed
    if(mySerial.read() == 97) //check recieve key from phone
    {
      Serial.println("succeed!");
      for(int i=0;i<2;i++){
          mySerial.write(packet1[i]);
      }
      
      }
      }
   }
 else{
   analogWrite(trig1,LOW);
 }

     delay(10);
if (RED2==0){ 
    if(YU_light<=700){
      analogWrite(trig2,contact);
      analogWrite(3,LED_light);
    }
   else {
    analogWrite(trig2,LOW);
   }
    
   lcd.setCursor(4,0);
     lcd.print("B");
       sum2+=1;
       delay(1000);
      lcd.setCursor(4,1);
      lcd.print(sum2,DEC);
  byte packet2[2];
  packet2[0]= 96;
  packet2[1]=sum2;
  Serial.println(sum2);
    if(mySerial.available() > 0){//check BT is succeed
    if(mySerial.read() == 96) //check recieve key from phone
    {
      Serial.println("succeed2!");
      for(int j=0;j<2;j++){
          mySerial.write(packet2[j]);
      }
      
      }
      }
}   
 else{
  analogWrite(trig2,LOW);
  }
 
      delay(10);
if ( RED3 == 0){
     if(YU_light<=700){
      analogWrite(trig3,contact);
      analogWrite(4,LED_light);
    }
   else {
    analogWrite(trig3,LOW);
   }
   
   lcd.setCursor(8,0);
     lcd.print("C");
       sum3+=1;
       delay(700);
      lcd.setCursor(8,1);
      lcd.print(sum3,DEC);
 }
 else{
   analogWrite(trig3,LOW);
 }
       delay(10);
 if ( RED4 == 0){  
     if(YU_light<700){
      analogWrite(trig4,contact);
      analogWrite(5,LED_light);
    }
   else {
    analogWrite(trig4,LOW);
  }
  lcd.setCursor(12,0);
     lcd.print("D");
       sum4+=1;
       delay(700);
      lcd.setCursor(12,1);
      lcd.print(sum4,DEC);
  delay(10);
 }
 else{
   analogWrite(trig4,LOW);
   }

}
