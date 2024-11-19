# Vehicle-accident-detection-system-
In this modern world, the increased usage of vehicles on roads has led to a higher likelihood of accidents. Unfortunately, many lives are lost due to delayed response and a lack of awareness regarding the causes of accidents. The Vehicle Accident Detection System aims to address this issue by promptly recognizing accidents and alerting emergency services such as ambulances and the police.


This system involves data processing, location sharing, and analyzing accident causes. Its primary objective is to automatically identify and classify potential vehicle accidents based on various parameters, including sudden deceleration, abrupt changes in vehicle orientation, and collision impact. Through the utilization of computer vision techniques, the system analyzes real-time video streams from strategically positioned cameras installed across road networks. The collected visual data is then processed, employing machine learning algorithms to accurately identify accident-related patterns and differentiate them from normal traffic scenarios.

A vehicle accident detection system is a technology designed to automatically detect and respond to vehicle accidents. It uses various sensors, algorithms, and communication systems to quickly identify when a vehicle has been involved in a collision or any other type of accident. The main purpose of such a system is to improve safety on the road by providing timely assistance to accident victims and alerting emergency services.
	  	     This vehicle accident detection system makes the life more responsible and gives effective progress regarding the road transport.This system can guarantee  to save the life species.The vehicle accident detection system is comes under the category of necessary systeem in our daily livelihood.
			The system involves various stages in execution of the project that will be explained in coming topics.The System can explains the occurrence of accident and the victim or the emergency servants or the vehicle manufacture industries can be guided by the information that has been provided by the system.

The aim of the Vehicle Accident Detection System project is to improve road safety and minimize the impact of accidents by providing timely assistance to accident victims. And the primary and necessary system for today's modern world to save the life.


#include <SoftwareSerial.h>

#define RX 10

#define TX 11

#define sensorPin A0

int m=0;

String phno = "+919xxxxxxxxx";
String res = " ";
char response[30] = " ";
SoftwareSerial A9GSerial(RX, TX);
void setup() {
  pinMode(sensorPin, INPUT);
  Serial.begin(115200);
  A9GSerial.begin(115200);
  delay(20000);
  A9GSerial.println("AT");
  delay(1000);
  A9GSerial.println("AT+GPS=1");
  delay(1000);
  A9GSerial.println("AT+CPIN?");
  delay(1000);
}
void loop() {
  double voltage = analogRead(sensorPin) ;
  if (voltage > 25 || (A9GSerial.available() && A9GSerial.readString() == "SEND LOCATION")) {
    // Code block to execute if the condition is true
    Serial.println("Voltage is " + String(voltage, 2)); 
    A9GSerial.println("AT+LOCATION=2");
    Serial.println("AT+LOCATION=2");
    while (!A9GSerial.available())
      ;
    while (A9GSerial.available()) {
      char add = A9GSerial.read();
      res += add;
      delay(1);
    }
    res = res.substring(17, 38);
    strcpy(response, res.c_str());
    Serial.print("Received Data - ");
    Serial.println(response);
    Serial.println();
    if (strstr(response, "GPS NOT")) {
      Serial.println("No location data");
    } else {
      int i = 0;
      while (response[i] != ',')
        i++;
      String location = String(response);
      String lat = location.substring(2, i);
      String longi = location.substring(i + 1);
      Serial.println(lat);
      Serial.println(longi);
      String Gmaps_link = "http://maps.google.com/maps?q=" + lat + "+" + longi;
      A9GSerial.println("AT+CMGF=1");
      delay(1000);
      A9GSerial.println("AT+CMGS=\"" + phno + "\"\r");
      delay(1000);
      A9GSerial.println("ACCIDENT HAS OCCURRED. PLEASE RESCUE " + Gmaps_link);
      delay(1000);
      A9GSerial.write(0x1A); // it is command for endimng for the message
      delay(1000);
    }
    response[0] = '\0';
    res = "";
    while(m<2)
    {
    Serial.println("Calling Now");
    A9GSerial.println("ATD" + phno);
    m++;
    }
  }
