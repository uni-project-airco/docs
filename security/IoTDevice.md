To ensure the security and integrity of the SafeAir IoT device, several protective measures are implemented across the hardware and communication layers.

+ Device Authentication:
Each Raspberry Pi authenticates using a unique certification string before it is allowed to register and receive its private PubNub channel.

+ Secure Communication:
All communication with the backend server uses HTTPS, preventing interception or tampering of sensor data or configuration values.

+ Controlled PubNub Access:
PubNub channels are protected using publish/subscribe keys and access policies, ensuring only verified devices can publish alerts and only trusted clients can subscribe.