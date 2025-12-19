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


Github workflow

Auto Deploy Flask on EC2

This GitHub Actions workflow deploys a Flask app to an EC2 instance using Docker Compose whenever code is pushed to the main branch.
name: Auto Deploy Flask on EC2

on:
  push:
    branches: ["main"]
Runs automatically on every push to main.
------------------------------------------------------------------------------------------------------------------------------------
What the workflow does
1. Checkout the repository
   - uses: actions/checkout@v4
     
------------------------------------------------------------------------------------------------------------------------------------
2. Copy files to EC2
   - uses: appleboy/scp-action@v0.1.5
------------------------------------------------------------------------------------------------------------------------------------

3. Copies the entire repository to the EC2 server via SCP.

Files are placed in:

/home/<SERVER_USER>/app
Uses SSH credentials stored in GitHub secrets.
------------------------------------------------------------------------------------------------------------------------------------
3. Deploy on EC2
- uses: appleboy/ssh-action@v1.1.0
  Connects to the EC2 instance and runs:
      cd /home/<SERVER_USER>/app
      sudo docker compose up --build -d
  This rebuilds Docker images and restarts the containers in detached mode.

------------------------------------------------------------------------------------------------------------------------------------
Required GitHub secrets

Set these in Settings → Secrets and variables → Actions:
| Name             | Description                  |
| ---------------- | ---------------------------- |
| `SERVER_IP`      | EC2 public IP                |
| `SERVER_USER`    | SSH username (e.g. `ubuntu`) |
| `SERVER_SSH_KEY` | Private SSH key              |

------------------------------------------------------------------------------------------------------------------------------------
EC2 requirements

The EC2 instance must have:

1. Docker installed
2. Docker Compose installed
3. SSH access enabled
4. docker-compose.yml in the project root
--------------------------------------------------------------------------------------------------------------------------------------


