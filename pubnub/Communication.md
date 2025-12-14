## Communication
#### Hardware
In SafeAir we use PubNub to implement real time notifications. The hardware (Raspberry pi) gathers the data (Temperaure, C02, Humidity, PM2.5), compares to the thresholds set on sensor, and if any sensor is not within the normal diapazone - publishes a structured alert message—including a title, message text, status level, and timestamp—to a secured PubNub channel.
<br>

#### Frontend
On the client side, the Android app subscribes to this channel through PubNub's Kotlin SDK. A <b>SubscribeCallback</b> is implemented to read the message and parse it, transforming the message into a notification within the app, allowing the user to see it instantly without having to refresh or update. Thus, whenever air quality is detected as unsafe, the user knows immediately. 
<br>

#### Backend
On the backend we tried to use PubNub to trasnfer the data from sensors to flask and proceed it afterwards. We were implemeting the system, where PubNub hits the enpoint on backend, sending the "string certificate". Flask validates the certificate, and if everything fine, generates a secure channel name, using the preconfigured sensor_id as a salt, to add an extra layer of security, and hashing afterwards. Unfortunately, the system did not work well, because the channel name was not assigned properly. That is why we had to use different logic for now. We are sending data every 10 minutes using the endpoints, end then managing its distribution in databases (for different time periods) using cron.