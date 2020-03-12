//Temperature code :start

int ThermistorPin = 0;
const int led_pin_temperature =12; int Vo;
float R1 = 10000;
float logR2, R2, T, Tc, Tf;
float c1 = 1.009249522e-03, c2 = 2.378405444e-04, c3 = 2.019202697e-07;

//Temperature : End

//Relay : Start

const int relay1 = 6; //Arduino pin that triggers relay #1 const int relay2 = 7; //Arduino pin that triggers relay #2 const int relay3 = 8; //Arduino pin that triggers relay #3 const int relay4 = 9; //Arduino pin that triggers relay #4

//Relay : End
// vibration sensor int vs =10;
const int led_pin_vibration = 13;

// Date and time functions using a DS3231 RTC connected via I2C and Wire lib #include <Wire.h>
#include "RTClib.h" RTC_DS3231 rtc;

int  h,m,z; String t = ""; String hstr,mstr;

long vibration();

//Setup
void setup () {
 
pinMode(led_pin_temperature, OUTPUT); pinMode(led_pin_vibration, OUTPUT); pinMode(vs, INPUT);

//relay : Start pinMode(relay1, OUTPUT); pinMode(relay2, OUTPUT); pinMode(relay3, OUTPUT); pinMode(relay4, OUTPUT);
//relay : End

#ifndef ESP8266
while (!Serial); // for Leonardo/Micro/Zero #endif

Serial.begin(9600);

delay(3000); // wait for console opening

if (! rtc.begin()) { Serial.println("Couldn't find RTC"); while (1);
}
rtc.adjust(DateTime(F( DATE ), F( TIME__)));


if (rtc.lostPower()) {
Serial.println("RTC lost power, lets set the time!");
// following line sets the RTC to the date & time this sketch was compiled rtc.adjust(DateTime(F( DATE ), F( TIME__)));
// This line sets the RTC with an explicit date & time, for example to set
// January 21, 2014 at 3am you would call:
// rtc.adjust(DateTime(2014, 1, 21, 3, 0, 0));
}
}

//Setup End
 
//Relay : Start
void extendActuator1() { digitalWrite(relay1, HIGH); digitalWrite(relay2, LOW); digitalWrite(relay3, LOW); digitalWrite(relay4, LOW);

}


void extendActuator2() { digitalWrite(relay1, LOW); digitalWrite(relay2, LOW); digitalWrite(relay3, HIGH); digitalWrite(relay4, LOW);
}


void retractActuator1() { digitalWrite(relay1, LOW); digitalWrite(relay2, HIGH); digitalWrite(relay3, LOW); digitalWrite(relay4, LOW);

}


void retractActuator2() { digitalWrite(relay1, LOW); digitalWrite(relay2, LOW); digitalWrite(relay3, LOW); digitalWrite(relay4, HIGH);
}
void stopActuator() { digitalWrite(relay1, LOW); digitalWrite(relay2, LOW);

digitalWrite(relay3, LOW); digitalWrite(relay4, LOW);
}
 
//Relay : End


void loop() {


//vibration : Start

long measurement=vibration(); Serial.println(measurement);

//vibration : End

//RTC: Start

DateTime now = rtc.now(); h=now.hour(); m=now.minute();
hstr = String(h); mstr = String(m); t += hstr;
t += mstr;
z = t.toInt();

//RTC : End


//Temperature: Start

Vo = analogRead(ThermistorPin); R2 = R1 * (1023.0 / (float)Vo - 1.0);
logR2 = log(R2);
T = (1.0 / (c1 + c2*logR2 + c3*logR2*logR2*logR2)); Tc = T - 273.15;
Serial.println(Tc);
// Temperature : End

if (Tc<80 && measurement<20000)
{
digitalWrite(led_pin_vibration,LOW); digitalWrite(led_pin_temperature,LOW);
 
Serial.println(z); switch (z)
{
case 80:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(17000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(75000); stopActuator(); delay(1000); break;
}
case 830:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(21000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(70000); stopActuator(); delay(1000);
 
break;
}
case 90:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(25000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(65000); stopActuator(); delay(1000); break;
}
case 930:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(27000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(60000); stopActuator(); delay(1000); break;
 
}
case 100:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(29500); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(56500); stopActuator(); delay(1000); break;
}
case 1030:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(33000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(53000); stopActuator(); delay(1000); break;
}
 
case 110:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(36000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(50000); stopActuator(); delay(1000); break;
}
case 1130:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(39000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(46000); stopActuator(); delay(1000); break;
}
case 120:
 
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(42600); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(42600); stopActuator(); delay(1000); break;
}
case 1230:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(46000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(39000); stopActuator(); delay(1000); break;
}
case 130:
{
 
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(49500); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(36000); stopActuator(); delay(1000); break;
}
case 1330:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(53000); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(32700); stopActuator(); delay(1000); break;
}
case 140:
{
retractActuator1();
 
delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(56400); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(29300); stopActuator(); delay(1000); break;
}
case 1430:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(60300); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(27000); stopActuator(); delay(1000); break;
}
case 150:
{
retractActuator1(); delay(120000);
 
stopActuator(); delay(2000); extendActuator1(); delay(64500); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(24500); stopActuator(); delay(1000); break;
}
case 1530:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); extendActuator1(); delay(69300); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(20800); stopActuator(); delay(1000); break;
}
case 160:
{
retractActuator1(); delay(120000); stopActuator();
 
delay(2000); extendActuator1(); delay(74800); stopActuator(); delay(1000); retractActuator2(); delay(120000); stopActuator(); delay(1000); extendActuator2(); delay(16700); stopActuator(); delay(1000); break;
}
case 1630:
{
retractActuator1(); delay(120000); stopActuator(); delay(2000); retractActuator2(); delay(120000); stopActuator(); delay(1000); break;
}
}
}
else if (Tc>=80 || measurement>=20000)
{
if (Tc>=80)
{
digitalWrite(led_pin_temperature,HIGH);
}
else if (measurement>=5000)
{
digitalWrite(led_pin_vibration,HIGH);
}

retractActuator1();
 
delay(120000); stopActuator(); delay(3000);

retractActuator2(); delay(120000); stopActuator(); delay(3000);
}

t = "";

}



// vibration : Start long vibration()
{
long measurement=pulseIn(vs, HIGH);
//wait for the pin to get HIGH and returns measurement return measurement;
}
// vibration : End /*
