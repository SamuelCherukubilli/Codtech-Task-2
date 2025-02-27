# Codtech-Task-2

Home Automation System with IoT Platform (Blynk or MQTT)
The goal is to develop a home automation prototype that allows control of multiple devices (such as lights and fans) through an IoT platform like Blynk or MQTT. This system can be controlled via a smartphone or web application, providing remote and efficient control over home appliances.

We will design a prototype using Blynk, as it simplifies the process of connecting devices to the cloud and allows for easy control via a mobile app. Alternatively, you can use MQTT for more complex use cases where flexibility is needed (e.g., controlling multiple devices over the internet).

Components Required:
Microcontroller:

ESP8266 or ESP32 (Wi-Fi enabled microcontrollers, these are ideal for IoT applications).
Relays:

4-channel relay module to control multiple devices (lights, fans, etc.).
Devices:

LEDs or Light Bulbs (for lights).
Fans (or other appliances).
Power Supply:

For the microcontroller and relays (ensure correct voltage).
Smartphone/PC:

Blynk mobile app or a web app to control devices.
Alternatively, if using MQTT, you can also use a custom app or web interface.
System Overview:
ESP8266/ESP32 will act as the IoT gateway, connecting to Wi-Fi and communicating with the cloud platform (Blynk or MQTT).
Relays will control the devices (lights, fans), turning them on or off.
Blynk or MQTT will handle remote control from a smartphone or web app.
System Design:
Step 1: Hardware Setup
Connect the Relays to the Microcontroller:

Connect each relay's input pin to a GPIO pin on the ESP8266 or ESP32.
Connect the common terminals (COM) of the relays to the power supply for the devices (lights/fans).
Connect the Normally Open (NO) terminal of each relay to the device you wish to control (e.g., light or fan).
Ensure the relay is rated for the device’s voltage and current.
Connect the ESP8266/ESP32 to the Wi-Fi Network:

Make sure your ESP device is connected to your home Wi-Fi network. This will enable communication between the ESP and the Blynk or MQTT platform.
Step 2: Setting Up Blynk (for Simplicity)
Install the Blynk App:

Download and install the Blynk app on your smartphone from Google Play or the App Store.
Create a New Project in the Blynk App:

Select your device (ESP8266/ESP32).
Choose the connection type (Wi-Fi).
Generate an authentication token, which will be sent to your email (you’ll need this for your Arduino code).
Add Widgets in the Blynk App:

Add Button widgets to control the lights and fans.
Set the button to "Switch" mode, so it can toggle the device on and off.
For each button, assign a virtual pin (e.g., V1 for light 1, V2 for fan, etc.).
Add a Timer Widget (Optional):

You can add a timer to automate actions, such as turning lights on/off after a set time.
Step 3: Arduino Code (ESP8266/ESP32)
Install the Blynk Library:

Open the Arduino IDE and install the Blynk library via the Library Manager.
Install the necessary libraries for ESP8266/ESP32.
Write the Arduino Code:

Here’s an example of the code to control multiple devices (light and fan) using Blynk:

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// Blynk authentication token
char auth[] = "YourAuthToken";  

// WiFi credentials
char ssid[] = "YourSSID";
char pass[] = "YourPassword";

// Pin definitions
const int lightPin = D1;  // GPIO pin for Light
const int fanPin = D2;    // GPIO pin for Fan

void setup() {
  // Start serial communication
  Serial.begin(9600);

  // Connect to Wi-Fi
  WiFi.begin(ssid, pass);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");

  // Initialize Blynk
  Blynk.begin(auth, ssid, pass);

  // Set device pins as output
  pinMode(lightPin, OUTPUT);
  pinMode(fanPin, OUTPUT);
}

BLYNK_WRITE(V1) {
  int lightState = param.asInt();  // Get state of the button (0 or 1)
  digitalWrite(lightPin, lightState);  // Control light
}

BLYNK_WRITE(V2) {
  int fanState = param.asInt();  // Get state of the button (0 or 1)
  digitalWrite(fanPin, fanState);  // Control fan
}

void loop() {
  Blynk.run();  // Run the Blynk app
}


Step 4: Testing the System
Upload the Code to ESP8266/ESP32:

Upload the code to the microcontroller using the Arduino IDE.
Test the Control:

Open the Blynk app on your smartphone.
Connect to the ESP8266/ESP32 via Wi-Fi (you should see it online if the connection is successful).
Press the ON/OFF buttons in the app to control the lights and fans.
Step 5: (Optional) Use MQTT for More Flexibility
If you prefer to use MQTT (a popular messaging protocol for IoT), here’s a brief guide:

Set up an MQTT Broker:

You can use Mosquitto (an open-source MQTT broker) or a cloud-based service like CloudMQTT.
Write MQTT Code for ESP8266/ESP32:

Use the PubSubClient library in the Arduino IDE for MQTT.
You’ll need to subscribe to topics for each device (e.g., home/light, home/fan), and when a message is received, control the corresponding relay.
Develop a Web or Mobile App:

You can use Node-RED, MQTT.fx, or a custom app to publish MQTT messages and control devices. In this case, you would publish messages like ON or OFF to the appropriate topics.
Conclusion:
This system allows you to control multiple devices (lights, fans) via a smartphone app (Blynk or MQTT). The ESP8266/ESP32 microcontroller connects to the cloud, where commands are received from the mobile app and passed to the relays, which control the devices. This setup provides a basic but expandable platform for home automation that can be further enhanced with more devices, automation rules, and web interfaces.

Enhancements:

Add sensors (motion, temperature) to automate control based on conditions.
Use Google Assistant or Amazon Alexa integration for voice control.
Create a dashboard using Blynk Web Dashboard for desktop control.
