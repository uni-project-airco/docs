We use MongoDB Atlas as our cloud database, and most of our security decisions revolve around making sure that nobody except our backend can access it.

1. Network Access Restrictions
The database isn’t open to the internet. In MongoDB Atlas we manually whitelist the IP address of our EC2 server, so it’s literally the only machine that can talk to the database. Even if someone somehow got our credentials, they wouldn’t be able to connect unless they were coming from that exact server.

2. User Accounts & Permissions
We created a separate database user just for our backend. It has a long, randomly-generated password, and its permissions are limited to only what our API actually needs. For example, our backend can insert or read telemetry data, but we didn’t give it rights to drop collections or do admin-level actions. Keeping privileges minimal protects the system if credentials ever leak.

3. Encryption at Rest
Atlas automatically encrypts everything using AES-256. We didn’t have to configure anything special — data on disk is encrypted by default. Even if someone magically got access to the storage layer, they wouldn’t be able to read the data.

4. Encrypted Connection (TLS/SSL)
All requests from the backend to MongoDB travel through an encrypted SSL/TLS connection. This protects the data while it’s moving and prevents anyone from sniffing it on the network.

5. No Direct IoT → Database Access
IoT devices never talk to MongoDB at all. They communicate only with our backend, which validates and sanitizes everything before storing it. This prevents malformed or malicious data from being written to the database directly.

6. Monitoring and Alerts
Atlas gives us built-in monitoring, so things like repeated failed logins or unusual query patterns trigger alerts. This helps us spot problems early.

Overall, the database is locked down from multiple angles: limited network access, encrypted storage, secure connections, and tightly controlled permissions.
