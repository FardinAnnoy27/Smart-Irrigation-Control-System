Smart Irrigation Control System

 
Fig: Plant keeper

The Problem Statement

Indoor plants play an important role to enhance living spaces by improving air quality, boosting mood, enriching indoor beauty. But after of all maintaining indoor plant is a difficult task for individual with busy lifestyle. To maintain work commitments, travel, or personal responsibilities, it’s difficult to ensure consistent plant care.

1.	Lack of Time for Regular Maintenance: Many indevedual leads busy life with balancing professional, academic, and personal obligations. It’s difficult for them to manage time to water the plant regularly, monitoring environmental conditions like temperature and humidity. This inconsistency may harm the plant health.

2.	Challenges During Travel or Extended Absences: Due to travel, vacation, or other reason indoor plants are often neglected, when we are from home. Plants may not  receive adequate water or care that is the result for dehydration, without anyone care.

3.	Manual Monitoring Difficulties: Caring of indoor plants is not only watering but also something else. Factors that is soil moisture levels, temperature  and humidity. Manual checking of this parameters is not only time consuming but also inaccurate.

4.	Risk of over and under watering: Different types of plant may have different watering needs based on their size and environment condition. Manually maintaining multiple plant can be difficult.

5.	Inefficient Water Usage: This conventional form of watering generally results in inefficient water usage, especially when people try to compensate for the loss of watering by over watering. This wastes not only water but also is destructive to plants and, most importantly, is non-environmentally sustainable.

6.	Existing Solutions Are Complex or Expensive: While some of the smart irrigation systems have entered the marketplace, many are either too expensive or too complicated to install and configure. In fact, most of them lack user-friendly interfaces with customization features by plant and are therefore out of reach for the average user.

The Objective

1.	Maintain Consistent Plant Care: Install an automated irrigation system which adjusts water delivery based on real-time soil moisture measurements to reduce over and underwatering.

2.	Enable Remote Monitoring and Control: Develop a user-friendly web interface for monitoring environmental conditions, including temperature, humidity, and soil moisture, and for remote watering control, which is especially useful when traveling or away from home.

3.	Provide Real-time Data Monitoring: Integrate sensors that measure soil moisture, temperature, and humidity continuously, to provide accurate and updated information for optimal plant health.

4.	Improve Water Efficiency: Minimise water wastage by adopting precision, sensor-based irrigation methods that ensure plants receive the required moisture without excess.

5.	Optimizing Plant Maintenance: Create an intuitive framework that simplifies the plant care process, making it accessible to people with minimal gardening experience.

6.	Provide Various Control Modes: Implement three different modes: Manual, Auto, and Timer. This would give users flexibility in managing their plants according to their needs and schedules.

7.	Promote Sustainable Living: Promote environmentally friendly practices through the conservation of water efficiency and minimizing resource wastage for sustainable indoor horticulture. Assures dependable system performance: Ensure the system is reliable and consistent in operation with clear status displays on a 20x4 LCD so users are always informed of the system's operations.

The Ultimate Solution

Component list:
1.	ESP 32 (1 Pcs)
                                    
2.	Big Bread Board (1 Pcs)
                                    
3.	Jumper Cable
                                                       
4.	DHT22 (1 Pcs)
                                                            
5.	Capacitative Soil Moisture Sensor (4 Pcs as We are working on 4 plants)
                                            
6.	20X4 LCD Display (1 Pcs)
                                        
7.	4 Channel Relay Module (1 Pcs as We are working on 4 plants)
                                         
8.	4 Water Pumps (4 Pcs as We are working on 4 plants)
                               
9.	Water contailer & Water Pipe (As Needed)
                                           

Project Description: This tutorial will guide you in building a Smart Irrigation Control System, Plant Keeper, using an ESP32 microcontroller, designed for automating indoor plant care. It monitors soil moisture, temperature, and humidity to keep your plants in optimum health even when you are not around. You will also learn how to connect the sensors—soil and DHT22—and control submersible pumps with a relay module, then display real-time data on a 20x4 LCD.
The tutorial also contains the design of the responsive web interface for remote monitoring and control, containing three modes of operation: Manual, Auto, and Timer. This tutorial shows you clear, step-by-step instructions with code and wiring diagrams to make your work friendly for beginners. From this course, students shall be able to design their fully operational smart irrigation system for proper water conservation and easier maintenance of plants—ensuring equal care among indoor vegetation.


Technical Description: 

ESP32: The ESP32 is a Wi-Fi and Bluetooth-enabled microcontroller for IoT applications. It has dual-core processors running at clock speeds of up to 240 MHz, executing multiple tasks at the same time. The ESP32 connects with sensors and devices via GPIO pins and supports several protocols like I2C, SPI, and UART.

It collects data from connected peripherals, processes this information, and can control outputs like motors or relays. The built-in Wi-Fi and Bluetooth functionalities provide remote communication and control over the device. Low-power modes enable the ESP32 to be energy efficient and so are suitable for battery-based projects. The programming becomes flexible using either Arduino IDE, ESP-IDF, or MicroPython—all these platforms make creating smart systems easier.

DHT22: The DHT22 measures temperature with a thermistor; it measures humidity with a capacitive sensor. Both sensors have their electrical properties changed by changes in temperature and humidity. These are picked up by an internal microcontroller, which processes these signals into digital data. It sends this data to the ESP32 or any other microcontroller through a single-wire communication protocol, hence providing accurate measurements for both temperature and humidity.

Capacitive Soil Sensor: A capacitive soil moisture sensor measures soil moisture by detecting changes in capacitance. As moisture increases, the dielectric constant of the soil rises, increasing the capacitance. This change is converted into an analog signal, which a microcontroller (like an ESP32) reads to determine soil moisture levels accurately without direct contact between the sensor and water.
Relay Module: A relay module is a kind of electrically operated switch. It makes use of a low-voltage signal to control a far larger electrical load. When the input signal coming from the microcontroller energizes the internal coil, a magnetic field will be generated, closing or opening the switch contacts—this way, allowing or interrupting the flow of high-power current through connected devices such as motors or lighting fixtures. This isolation mechanism assures safe control of the high-voltage devices by the low-voltage signals.

Dc pump: A DC mini pump uses a small direct current motor to drive either an impeller or diaphragm. When powered, the motor converts electrical energy into mechanical motion, creating suction that pulls water in and pushes it out through an outlet. Its compact size allows it to be used in applications where efficient water transfer is needed for small-scale irrigation or cooling systems.

Components	Connected with	Working Voltage
ESP32		5V
DHT22	ESP32 GPIO 26	3.3V
Capacitive Soil Moisture Sensor	ESP32 GPIO 32
ESP32 GPIO 33
ESP32 GPIO 34
ESP32 GPIO 35	3.3V
LCD Display	ESP32 GPIO 21
ESP32 GPIO 22	3.3V
Relay Module	ESP32 GPIO 13
ESP32 GPIO 14
ESP32 GPIO 27
ESP32 GPIO 15	5V
Motor	Relay channel 1
Relay channel 2
Relay channel 3
Relay channel 4	3.3V


Implements
Coding Part:
ESP32: 
------------------------------------------Header File---------------------------------------
#include <WiFi.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>
#include <FirebaseESP32.h>
-------------------Initialize the LCD with the I2C address and dimensions----------
LiquidCrystal_I2C lcd(0x27, 20, 4);
DHT dht(26, DHT22); // Initialize DHT sensor
--------------------------------------Pin Declearation ----------------------
const int sensorPin1 = 32;
const int sensorPin2 = 35;
const int sensorPin3 = 34;
const int sensorPin4 = 33;
--------------------------------------WiFi credentials----------------------
const char* ssid = "904-";
const char* password = "#1";
--------------------------------------Database credentials----------------
FirebaseConfig config;
FirebaseAuth auth;
#define FIREBASE_HOST "https://smart-irrigation--cp"
#define FIREBASE_API_KEY "-zGtkpekTMHhkfYs"

FirebaseData firebaseData;
----------------------------------------Setup Function----------------------
void setup() {
  Serial.begin(115200);
------------------------------------------Lcd Start---------------------------
lcd.init();
  lcd.backlight();
-----------------------------------------WIFI initialization-----------------
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  Serial.println("\nWi-Fi connected");
  lcd.print("Wi-Fi connected");
  -----------------------------------------Start DHT sensor--------------------
dht.begin();  
  ----------------------------------Configure Firebase------------------------------
  config.host = FIREBASE_HOST;
  config.api_key = FIREBASE_API_KEY;
  auth.user.email = "mdr8@gmail.com"; // If using email/password auth
  auth.user.password = "1213W#";
  ------------------------------Initialize Firebase---------------------------------------
  Firebase.begin(&config, &auth);
  Firebase.reconnectWiFi(true);
  --------------------------Test if Firebase is ready------------------------------------
  if (Firebase.ready()) {
    Serial.println("Firebase connected");
  } else {
    Serial.println("Failed to connect to Firebase");
  }
}
void loop() {
  lcd.clear();
---------------------------------------------Read temperature-----------------------------
  float temp = dht.readTemperature();
-------------------------------------------- Read humidity --------------------------------  
  float humi = dht.readHumidity();     
-------------------------------------------sent data to firebase----------------------------
  if (Firebase.setFloat(firebaseData, "/Temp", temp)) {
  Serial.println("Temperature sent to Firebase");
  } else {
    Serial.print("Failed to send temperature: ");
    Serial.println(firebaseData.errorReason());
  }
  if (Firebase.setFloat(firebaseData, "/Humidity", hum)) {
    Serial.println("Humidity sent to Firebase");
  } else {
    Serial.print("Failed to send humidity: ");
    Serial.println(firebaseData.errorReason());
  }
---------------------------------------------calculate percent------------------------------
  float sensorValue1 = 100.00 - ((analogRead(sensorPin1) - 1040.00) * 100.00 / (2860.00 - 1040.00));
  float sensorValue2 = 100.00 - ((analogRead(sensorPin2) - 1010.00) * 100.00 / (2800.00 - 1010.00));
  float sensorValue3 = 100.00 - ((analogRead(sensorPin3) - 1110.00) * 100.00 / (2840.00 - 1110.00));
  float sensorValue4 = 100.00 - ((analogRead(sensorPin4) - 980.00) * 100.00 / (2640.00 - 980.00));
---------------------------------------set persent limit-----------------------------------
  sensorValue1 = constrain(sensorValue1, 0, 100);
  sensorValue2 = constrain(sensorValue2, 0, 100); 
  sensorValue3 = constrain(sensorValue3, 0, 100); 
  sensorValue4 = constrain(sensorValue4, 0, 100);
--------------------------------------Serial Monitor print------------------------------
  Serial.print("Temperature: ");
  Serial.println(temp);
  Serial.print("Humidity: ");
  Serial.println(humi);
  Serial.print("Muisture 1: ");
  Serial.println(sensorValue1);
  Serial.print("Muisture 2: ");
  Serial.println(sensorValue2);
  Serial.print("Muisture 3: ");
  Serial.println(sensorValue3);
  Serial.print("Muisture 4: ");
  Serial.println(sensorValue4);
--------------------------------------LCD Monitor print------------------------------
  lcd.setCursor(0, 0);
  lcd.print("T ");
  lcd.print(temp);
  lcd.print(" C ");

  lcd.print("H ");
  lcd.print(humi);
  lcd.print(" %");
  lcd.setCursor(0, 1);
  lcd.print(">");
  lcd.setCursor(1, 1);
  lcd.print(int(sensorValue1));
  lcd.print("%");
  lcd.setCursor(6, 1);
  lcd.print(int(sensorValue2));
  lcd.print("%");
  lcd.setCursor(11, 1);
  lcd.print(int(sensorValue3));
  lcd.print("%");
  lcd.setCursor(16, 1);
  lcd.print(int(sensorValue4));
  lcd.print("%");

  delay(2000); // Delay for 2 seconds
}

WebApplication:
              
With this application you can moitor the perameters and control your system. There are 3 operation mode: Manual, Auto, Timer.

Manual Mode: In Manual Mode user water the plant manually with this device.
Auto Mode: In Auto mode the device water the plant in the moisture range selected by user.
Timer Mode: In Timer Mode User can select a timer after that the system will water the plant repeatedly upto a moisture level.

This is one of the key feature of the system. This enable user to contol and monitor remotely.

Here is the Link for detailed code: [Plant Keeper](https://github.com/FardinAnnoy27/Smart-Irrigation-Control-System.git)

Conculation: The Smart Irrigation Control System provides an efficient and automated way to take care of plants, dealing with the challenges of having indoor plants during busy times or while away. Equipped with sensors, an ESP32 microcontroller, and a dynamic web interface, the system continuously monitors soil moisture, temperature, and humidity in real time. Its three operating modes—Manual, Auto, and Timer—render flexibility and reliability, thus assuring minimal water wastage and preventing the neglect of plants. This project doesn't just make indoor gardening easier; it's also a good example of the practical use of IoT technology that can be applied ideally to any smart home or plant enthusiasts.

