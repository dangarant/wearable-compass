# wearable-compass
An Arduino sketch for a wearable device (helmet for example) that reacts to North/South direction with vibration. By David Mackie and Daniel Garcia.
http://danigarcia.tilda.ws/whatyouhumanscallnorth

/*
  Arduino LSM9DS1 - Simple Magnetometer

  This example reads the magnetic field values from the LSM9DS1
  sensor and continuously prints them to the Serial Monitor
  or Serial Plotter.

  The circuit:
  - Arduino Nano 33 BLE Sense

  created 10 Jul 2019
  by Riccardo Rizzo

  This example code is in the public domain.
*/

#include <Arduino_LSM9DS1.h>

void setup() {
  
  pinMode(D3, OUTPUT);
  Serial.begin(9600);

  
  Serial.println("Started");

  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");
    while (1);
  }
  Serial.print("Magnetic field sample rate = ");
  Serial.print(IMU.magneticFieldSampleRate());
  Serial.println(" uT");
  Serial.println();
  Serial.println("Magnetic Field in uT");
  Serial.println("X\tY\tZ");
}

void loop() {
  float x, y, z;

  if (IMU.magneticFieldAvailable()) {
    IMU.readMagneticField(x, y, z);

    Serial.print(x);
    Serial.print('\t');
    Serial.print(y);
    Serial.print('\t');
    Serial.println(z);
  }

  float heading = (atan2(y,x) * 180) / 3.14159;
  Serial.print('\t');
  Serial.println(heading);


    if ((heading < 30 && heading > 0) || (heading < -150))
    
    {
      digitalWrite(D3, HIGH);
     
    }
    else{
      digitalWrite(D3, LOW);
    }

    
   
//    if ( (x < 15) && (x > -15) && (y > -5) && (y < 0) ) 
//    
//    {
//      digitalWrite(D3, HIGH);
//     
//    }
//    else{
//      digitalWrite(D3, LOW);
//    }
  delay(100);
}
