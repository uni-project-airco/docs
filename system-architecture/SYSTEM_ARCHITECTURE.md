# System Architecture


## System diagram
<p align="center">
    <img src="./assets/system_architecture.png">
</p>

### Data flow

The diagram shows how our system exchanges data between sensors, the backend server, and the frontend application.
We have two different data transfer routes: one for alerts and one for history.

#### **1. Sensor operation and sending alert notifications (via PubNub)**

Each sensor on the device operates according to the following principle:

1. Every 2 seconds the sensor reads a new value (temperature, humidity, CO₂, and dust).
2. The sensor compares the received value with the acceptable limit.
3. If the value is normal, it sends nothing.
4. If the value is above the threshold, the sensor generates an alert.
5. This alert is sent via PubNub to the appropriate channel.

PubNub sends this alert to the mobile app (frontend). The app receives the message and displays a notification to the user that an excess has been detected (for example, high CO₂ levels).
In the diagram, this is the bottom path: sensor → PubNub → app.

#### **2.Sending average values ​​to the Backend (every 10 minutes)**

In addition to alert notifications, the sensor records data needed for history and analytics.

1. The sensor collects all measured data over 10 minutes.
2. It then calculates the average value for each indicator: temperature, humidity, dust, CO₂, etc.
3. The device sends these average values ​​to the backend via HTTPS every 10 minutes.
4. The backend receives the information and verifies its accuracy.
5. If the data is valid, the server stores it in MongoDB.

This part is shown on the top line of the diagram: 
Sensors → HTTPS → Backend → MongoDB.
The saved data is then used to display the history.

## Hardware diagram
<p align="center">
    <img src="./assets/hardware-configuration-2.png">
</p>

### Required parts
- Raspberry Pi 5
- Buzzer
- Adafruit particle sensor PMSA003I 
- Adafruit CO2 sensor SCD-41
- Button
