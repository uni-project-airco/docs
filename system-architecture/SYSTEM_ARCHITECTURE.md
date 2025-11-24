# System Architecture


## System diagram
<p align="center">
    <img src="./assets/system_architecture.png">
</p>

### Data flow

The diagram shows how our system exchanges data between sensors, the backend server, and the frontend application.
We have two different data transfer routes: one for alerts and one for history.

####**1. Sensor operation and sending alert notifications (via PubNub)**
Each sensor on the device operates according to the following principle:
1. Every 2 seconds the sensor reads a new value (temperature, humidity, CO₂, and dust).
2. The sensor compares the received value with the acceptable limit.
3. If the value is normal, it sends nothing.
4. If the value is above the threshold, the sensor generates an alert.
5. This alert is sent via PubNub to the appropriate channel.

PubNub sends this alert to the mobile app (frontend). The app receives the message and displays a notification to the user that an excess has been detected (for example, high CO₂ levels).

In the diagram, this is the bottom path: sensor → PubNub → app.

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
