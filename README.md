# 🌿 Smart Irrigation Control System — Plant Keeper 🌿

The **Smart Irrigation Control System (Plant Keeper)** is a modern IoT-driven ecosystem engineered using the ESP32 microcontroller to revolutionize indoor plant care. By monitoring critical environmental telemetry like soil moisture, temperature, and humidity in real-time, it ensures absolute hydration accuracy. Features a responsive web dashboard for remote infrastructure management under three smart execution presets: Manual, Auto, and Timer.

---

## 📸 System Overview
https://github.com/FardinAnnoy27/Smart-Irrigation-Control-System.git
---

## ⚠️ The Problem Statement

Indoor vegetation plays a crucial role in purifying air quality, minimizing stress, and boosting aesthetics. However, active individuals face significant hurdles in maintaining plant life:

* 📌 **1. Chronic Lack of Time:** Academic, corporate, and household obligations make it nearly impossible to inspect microclimatic conditions manually on a regular basis.
* 📌 **2. Neglect During Travel & Vacation:** When homeowners are away for extended durations, plants are left unattended, leading to rapid dehydration and death.
* 📌 **3. Faulty Manual Monitoring:** Guessing a plant's moisture needs by touch is inaccurate, time-consuming, and highly prone to human error.
* 📌 **4. Risks of Volumetric Disproportion:** Different plants require custom watering volumes. Blindly watering multiple species leads to dangerous over-watering or under-watering.
* 📌 **5. Wasteful Resource Consumption:** Conventional open-loop watering leads to heavy fluid runoff, which is neither cost-effective nor environmentally sustainable.
* 📌 **6. Expensive Proprietary Solutions:** Commercial smart irrigation rigs in the market are either too costly or overly complex for the average consumer to deploy.

---

## 🎯 The Core Objectives

* 💧 **Maintain Automated Consistency:** Deploy an intelligent system that delivers precision hydration based on live capacitive feedback loop.
* 🌐 **Enable Global Cloud Access:** Build an integrated web UI to check soil/climate status and toggle remote overrides from anywhere in the world.
* 📊 **Stream Real-Time Data:** Continuously ingest accurate telemetry to help users keep track of optimal vegetation health.
* ♻️ **Maximize Water Efficiency:** Prevent environmental waste through sensor-based precision gating systems.
* 🌱 **Simplify Indoor Gardening:** Democratize smart agriculture by providing an intuitive system architecture built for beginners.
* ⚙️ **Provide Flexible Control Presets:** Deliver three specialized operational run-modes (Manual, Automated, and Timer iterations).
* 📟 **Ensure Diagnostic Transparency:** Provide instantaneous local monitoring on-site using a high-density 20x4 Alphanumeric LCD panel.

---

## 🛠️ Hardware Solution & Component List

* 🧠 **ESP32 Microcontroller Module** (1 Pcs)
* 🔌 **Large Solderless Breadboard** (1 Pcs)
* ⚡ **Premium Multi-strand Jumper Cables** (As needed)
* 🌡️ **DHT22 Temperature & Humidity Sensor** (1 Pcs)
* 🔋 **Capacitive Soil Moisture Sensors** (4 Pcs — assigned to 4 isolated plants)
* 📺 **20x4 Alphanumeric LCD Module with I2C Backplane** (1 Pcs)
* 🎛️ **4-Channel Isolated Relay Module** (1 Pcs)
* 🌊 **5V DC Mini Submersible Water Pumps** (4 Pcs)
* 🪣 **Fluid Reservoir & Delivery Conduits/Pipes** (As needed)

---

## ⚙️ Hardware Architecture & Interfacing

### 📌 Master Pin Mapping & Voltage Matrix

| Component Name | Interfacing MCU Pin | Logic/Operating Voltage | Functional Domain |
| :--- | :--- | :--- | :--- |
| **ESP32 Core Processor** | Main Power Inlet | 5V Input | Central Compute & Cloud Gateway Routing |
| **DHT22 Sensor** | ESP32 GPIO 26 | 3.3V | Ambient Air Temp & Relative Humidity Data |
| **Capacitive Sensor 1** | ESP32 GPIO 32 | 3.3V | Localized Moisture Metrics for Plant 1 |
| **Capacitive Sensor 2** | ESP32 GPIO 35 | 3.3V | Localized Moisture Metrics for Plant 2 |
| **Capacitive Sensor 3** | ESP32 GPIO 34 | 3.3V | Localized Moisture Metrics for Plant 3 |
| **Capacitive Sensor 4** | ESP32 GPIO 33 | 3.3V | Localized Moisture Metrics for Plant 4 |
| **I2C LCD Display (20x4)**| GPIO 21 (SDA) / 22 (SCL) | 5V / 3.3V | On-site Real-time Telemetry Dashboard |
| **Relay Channel 1** | ESP32 GPIO 13 | 5V Logic | Power Switching Gate for Pump 1 |
| **Relay Channel 2** | ESP32 GPIO 14 | 5V Logic | Power Switching Gate for Pump 2 |
| **Relay Channel 3** | ESP32 GPIO 27 | 5V Logic | Power Switching Gate for Pump 3 |
| **Relay Channel 4** | ESP32 GPIO 15 | 5V Logic | Power Switching Gate for Pump 4 |
| **Submersible Motors** | Target Relay Terminals | 3.3V - 5V | Fluid Displacement & Direct Soil Irrigation |

---

## 💻 Core Firmware Implementation

### 📜 ESP32 Embedded Source Code (`PlantKeeper.ino`)

```cpp
// ------------------------------------------ Header Files ---------------------------------------
#include <WiFi.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>
#include <FirebaseESP32.h>

// ------------------- Initialize the LCD with the I2C address and dimensions ----------
LiquidCrystal_I2C lcd(0x27, 20, 4);
DHT dht(26, DHT22); // Initialize DHT sensor on GPIO 26

// -------------------------------------- Pin Declaration ----------------------
const int sensorPin1 = 32;
const int sensorPin2 = 35;
const int sensorPin3 = 34;
const int sensorPin4 = 33;

// -------------------------------------- WiFi Credentials ----------------------
const char* ssid = "904-";
const char* password = "#1";

// -------------------------------------- Database Credentials ----------------
FirebaseConfig config;
FirebaseAuth auth;
#define FIREBASE_HOST "https://smart-irrigation--cp"
#define FIREBASE_API_KEY "-zGtkpekTMHhkfYs"

FirebaseData firebaseData;

// ---------------------------------------- Setup Function ----------------------
void setup() {
  Serial.begin(115200);

  // ------------------------------------------ LCD Start ---------------------------
  lcd.init();
  lcd.backlight();
  lcd.print("Booting System...");

  // ----------------------------------------- WiFi Initialization -----------------
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
  Serial.println("\nWi-Fi connected");
  lcd.clear();
  lcd.print("Wi-Fi Connected");
  
  // ----------------------------------------- Start DHT Sensor --------------------
  dht.begin();  

  // ---------------------------------- Configure Firebase ------------------------------
  config.host = FIREBASE_HOST;
  config.api_key = FIREBASE_API_KEY;
  auth.user.email = "mdr8@gmail.com"; // System Authentication Email
  auth.user.password = "1213W#";

  // ------------------------------ Initialize Firebase ---------------------------------------
  Firebase.begin(&config, &auth);
  Firebase.reconnectWiFi(true);

  // -------------------------- Test if Firebase is Ready ------------------------------------
  if (Firebase.ready()) {
    Serial.println("Firebase connection established.");
  } else {
    Serial.println("Failed to connect to Firebase cloud gateway.");
  }
}

void loop() {
  // --------------------------------------------- Read Climate Data -----------------------------
  float temp = dht.readTemperature();
  float humi = dht.readHumidity();     

  // ------------------------------------------- Send Data to Firebase ----------------------------
  if (Firebase.setFloat(firebaseData, "/Temp", temp)) {
    Serial.println("Temperature uploaded successfully.");
  } else {
    Serial.print("Failed to send temperature: ");
    Serial.println(firebaseData.errorReason());
  }

  if (Firebase.setFloat(firebaseData, "/Humidity", humi)) {
    Serial.println("Humidity uploaded successfully.");
  } else {
    Serial.print("Failed to send humidity: ");
    Serial.println(firebaseData.errorReason());
  }

  // --------------------------------------------- Calculate Percentages ------------------------------
  // Maps raw analog values from Capacitive Sensors to clear 0-100% scales
  float sensorValue1 = 100.00 - ((analogRead(sensorPin1) - 1040.00) * 100.00 / (2860.00 - 1040.00));
  float sensorValue2 = 100.00 - ((analogRead(sensorPin2) - 1010.00) * 100.00 / (2800.00 - 1010.00));
  float sensorValue3 = 100.00 - ((analogRead(sensorPin3) - 1110.00) * 100.00 / (2840.00 - 1110.00));
  float sensorValue4 = 100.00 - ((analogRead(sensorPin4) - 980.00) * 100.00 / (2640.00 - 980.00));

  // --------------------------------------- Set Constraint Limits -----------------------------------
  sensorValue1 = constrain(sensorValue1, 0, 100);
  sensorValue2 = constrain(sensorValue2, 0, 100); 
  sensorValue3 = constrain(sensorValue3, 0, 100); 
  sensorValue4 = constrain(sensorValue4, 0, 100);

  // -------------------------------------- Serial Monitor Print ------------------------------
  Serial.print("Temperature: "); Serial.println(temp);
  Serial.print("Humidity: ");    Serial.println(humi);
  Serial.print("Moisture 1: ");  Serial.println(sensorValue1);
  Serial.print("Moisture 2: ");  Serial.println(sensorValue2);
  Serial.print("Moisture 3: ");  Serial.println(sensorValue3);
  Serial.print("Moisture 4: ");  Serial.println(sensorValue4);

  // -------------------------------------- LCD Panel Rendering ------------------------------
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("T:");
  lcd.print(temp, 1);
  lcd.print("C ");

  lcd.print("H:");
  lcd.print(humi, 1);
  lcd.print("%");

  lcd.setCursor(0, 1);
  lcd.print("P1:"); lcd.print(int(sensorValue1)); lcd.print("% ");
  lcd.print("P2:"); lcd.print(int(sensorValue2)); lcd.print("%");
  
  lcd.setCursor(0, 2);
  lcd.print("P3:"); lcd.print(int(sensorValue3)); lcd.print("% ");
  lcd.print("P4:"); lcd.print(int(sensorValue4)); lcd.print("%");

  delay(2000); // Master evaluation cycle pause
}
```
🌐 Responsive Web Application Control Modes
The ecosystem features a modern, user-friendly control dashboard designed to administer telemetry streams over the cloud. It operates under 3 primary modes:

🔴 Manual Override Mode: Provides total tactile freedom. Users can toggle individual digital buttons on the dashboard to irrigate target plants manually at will.

🟢 Automated Ingestion Mode: Works on an active sensor feedback script. The system continuously evaluates incoming moisture percentages and fires up the respective water pump if levels drop below user-configured limits.

🔵 Cyclic Timer Mode: Discharges fluids based on custom periodic time delays, running calculations systematically to avoid saturation limits.

🏁 Final Conclusion
The Smart Irrigation Control System (Plant Keeper) bridges the gap between urban living and nature preservation. By consolidating real-time climate tracking, Firebase IoT integrations, and multi-mode automated gating scripts, it successfully eradicates human error from plant maintenance. This repository serves as a highly scalable blueprint for future innovations in smart agriculture and residential automation networks.
