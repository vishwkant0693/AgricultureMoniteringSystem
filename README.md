# AgricultureMoniteringSystem

/*
Project by Salim Khazem from Algeria  
Github : salimkhazem 
*/
#include "DHT.h"

#define DHTPIN 4
#define DHTTYPE DHT11
DHT dht(DHTPIN,DHTTYPE); // define the type of DHT and the pin which is 4 and the type is DHT11
const int Sensor=0; //sensor assignment to pin analog 0
int Value; //declaration of the variable that will store the read data from the analog sensor 
const int relay1 = 12; //electrovanne pin  
const int relay2= 11;  //airmotor pin 
const int limit = 330;  // threshold for launching the solenoid valve
const byte ledgreen=8; 
const byte ledred = 7; 
 

void setup() {
 pinMode(Sensor,INPUT); //Put the sensor in Input  
 pinMode(relay1,OUTPUT); //put the relay in OUTPUT
 pinMode(relay2,OUTPUT); 
 pinMode(ledgreen,OUTPUT), pinMode(ledred,OUTPUT); 
 digitalWrite(relay1,LOW); 
 digitalWrite(relay2,HIGH);
 dht.begin();  //Start the temperature and humidity sensors  
 Serial.begin(9600); // Start the serial  


}

void loop() {
  float h = dht.readHumidity(); //  read the humidity value from sensor dht11
   float t= dht.readTemperature();  // read the temperature from the sensor dht11
   if (isnan(h) || isnan(t) ) {   // check if reading fails, if yes then leave the loop to start
    Serial.println("Failed");  //print the message Failed if the reading fails 
    return; 
   }
  delay(2000);  
   
  digitalWrite(ledgreen,LOW), digitalWrite(ledred,LOW) ;  // Initialize the led on off 
  Value=analogRead(Sensor); // Read the value from the sensor 
  if (Value>limit) {
    digitalWrite(relay1,LOW); //Launching the solenoid valve 
    digitalWrite(ledgreen,HIGH); // light up the green Led  
  }
  else if (Value<limit) {
    digitalWrite(relay1,HIGH); //Stop the solenoid valve 
    digitalWrite(ledred,HIGH);  //light up the red led 
  }
  if (t > 25) {
    digitalWrite(relay2,LOW) ; //Start the air motor 

  }
  else if (t<25) {
    digitalWrite(relay2,HIGH); // Stop the air motor 
  }
  Serial.print("Earth moisture:  "); 
  Serial.print("\t");
  Serial.print(Value); //print the value in the serial monitor 
  Serial.print("\t");
  Serial.print("Humidity of the air: "); 
  Serial.print(h);   // print the value of the humidity  of the sensor (dht)  in serial monitir 
  Serial.print("\t"); 
  Serial.print("Temperature: "); 
  Serial.println(t);   // print the value of temperature 
  
  delay(300); // delay between the reading data 

}

