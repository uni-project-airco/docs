Our backend is deployed on an AWS EC2 instance, and we use Apache as the reverse proxy in front of our Flask/Gunicorn server. The goal is to keep the server locked down so that the only things exposed to the public are the ports needed for the website.


1. Cloudflare + HTTPS Forced by Default

Our domain www.safe-air.org is managed through Cloudflare.
Cloudflare handles:

Automatic HTTPS

TLS termination

Certificate renewal

Redirecting all HTTP traffic to HTTPS

Hiding our actual EC2 IP

Basic DDoS and bot protection

So even before traffic reaches AWS, Cloudflare cleans and filters it.

On top of that, Apache also enforces an HTTPS redirect internally, so the connection is always encrypted, no matter where the request comes from.

2. Only Minimal Ports Open on EC2

Port 443 (HTTPS) → open to the public

SSH for CI/CD, SSM for manual deployment;
No other ports open at all

This reduces the attack surface massively.

3. Apache Reverse Proxy Layer

Apache acts as a buffer in front of the Python app.

This gives us:

control over which routes reach Gunicorn,

protection from direct access to the backend process,

the ability to filter or block unwanted requests,

clean routing from domain → app.

It also allows us to monitor logs and errors separately from the Python application.

4. Environment Variables for Secrets

None of our secrets — PubNub keys, MongoDB credentials, JWT secrets — are stored in the code.
They are stored securely on the EC2 instance using environment variables, which makes it harder for credentials to leak.
