# SafeAir – Documentation

Hosting on AWS EC2
Our backend is deployed on an AWS EC2 instance running Ubuntu. The application runs inside Docker, with Gunicorn handling the WSGI layer and Apache as a reverse proxy. Apache forwards HTTPS requests to Gunicorn via mod_proxy, isolating the backend while allowing secure public access.
Inbound traffic is controlled through AWS Security Groups, exposing only ports 80 (for redirect) and 443 (HTTPS). SSH access is restricted via AWS SSM Session Manager, so no public SSH port is exposed.
Custom Domain & Cloudflare
Our domain, www.safe-air.org is managed through Cloudflare:
    • DNS management is handled by Cloudflare
    • Automatic HTTPS redirection ensures all traffic uses TLS
    • Cloudflare acts as a CDN and reverse proxy, adding an extra layer of DDoS protection
    • The domain points to the EC2 instance via Route 53 A records, behind Cloudflare
This setup guarantees that users always access the site securely over HTTPS, and our backend remains hidden from direct public access.

