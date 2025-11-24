Our backend is deployed on an AWS EC2 instance, and we use Apache as the reverse proxy in front of our Flask/Gunicorn server. The goal is to keep the server locked down so that the only things exposed to the public are the ports needed for the website.

1. Secured Access via AWS SSM (No Public SSH)

One of the biggest security improvements in our setup is that we don’t use traditional SSH at all.
Instead, we manage the EC2 instance through AWS Systems Manager (SSM).

This gives us several benefits:

The EC2 instance has no public SSH port open, which removes a huge attack vector.

SSM sessions are authenticated through our AWS IAM account instead of SSH keys.

All sessions are encrypted and logged in CloudWatch by AWS automatically.

If someone scanned our EC2 IP, they wouldn't find SSH running at all.

This means the only real way to get into the machine is through AWS itself, which is far safer than exposing SSH to the internet.

2. Cloudflare + HTTPS Forced by Default

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

3. Only Minimal Ports Open on EC2

Because SSM replaces SSH, our EC2 security group is extremely strict:

Port 443 (HTTPS) → open to the public

Port 80 (HTTP) → only for redirect to HTTPS

No SSH (22)

No other ports open at all

This reduces the attack surface massively.

4. Apache Reverse Proxy Layer

Apache acts as a buffer in front of the Python app.

This gives us:

control over which routes reach Gunicorn,

protection from direct access to the backend process,

the ability to filter or block unwanted requests,

clean routing from domain → app.

It also allows us to monitor logs and errors separately from the Python application.

5. Environment Variables for Secrets

None of our secrets — PubNub keys, MongoDB credentials, JWT secrets — are stored in the code.
They are stored securely on the EC2 instance using environment variables, which makes it harder for credentials to leak.