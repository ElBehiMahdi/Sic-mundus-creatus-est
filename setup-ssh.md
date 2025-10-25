Arch Linux Server (192.168.0.130)
    ↓ SSH & Services
WSL Ubuntu (Management & Tunneling)
    ↓ Localhost forwarding
Windows Browser & CMD (User Access)


Option 1: Set Static IP on Arch Linux
On your Arch machine, configure a static IP:

bash
# Edit network configuration
sudo nano /etc/systemd/network/50-wired.network

Add:

text
[Match]
Name=en*

[Network]
Address=192.168.0.130/24
Gateway=192.168.0.1
DNS=192.168.0.1


Then restart network:

bash
sudo systemctl restart systemd-networkd


Add to Windows Hosts File
To use archserver hostname in Windows browser:

Open Notepad as Administrator

Open C:\Windows\System32\drivers\etc\hosts

Add this line:

text
192.168.0.130 archserver


Save the file

Now access from Windows browser:

text
http://archserver:8080



----------


✅ WHAT YOU SHOULD DO
1. Network Setup (Most Important)
Set static IP on Arch server via router DHCP reservation

Or configure static IP on Arch if you control the network

Verify network connectivity from both WSL and Windows

2. Name Resolution
In WSL Ubuntu:

bash
sudo nano /etc/hosts
Add 192.168.0.130 archserver


In Windows (optional but recommended):

Run Notepad as Admin

Edit C:\Windows\System32\drivers\etc\hosts

Add: 192.168.0.130 archserver

3. SSH Configuration (WSL → Arch)
Generate SSH keys in WSL:

bash
ssh-keygen -t ed25519 -C "wsl-to-arch"
ssh-copy-id mahdi@archserver

Create SSH config in WSL:

bash
nano ~/.ssh/config
text
Host archserver
    HostName archserver
    User mahdi
    Port 22
    IdentityFile ~/.ssh/id_ed25519


5. Firewall Configuration on Arch
bash
#Allow SSH
sudo ufw allow 22/tcp

#Only allow service ports locally (more secure)
sudo ufw allow from 192.168.0.0/24 to any port 22


❌ WHAT TO AVOID
1. Network Anti-Patterns
❌ Don't rely on dynamic DHCP - IP will change

❌ Don't bind services to 0.0.0.0 unnecessarily

❌ Don't disable firewalls completely

❌ Don't use password authentication for SSH

2. Security Anti-Patterns
❌ Don't expose services directly to your local network

❌ Don't run services as root

❌ Don't use default ports for sensitive services

❌ Don't skip SSH key authentication

3. Management Anti-Patterns
❌ Don't edit critical files without backups

❌ Don't mix WSL and Windows service management

❌ Don't forget to document your setup

🎯 IDEAL WORKFLOW
Daily Access:
Windows Browser: http://archserver:8080

WSL Terminal: ssh archserver for management

Keep it simple - no tunnels needed for basic access

Container Management (from WSL):
bash
# Connect to Arch server
ssh archserver

# Check status
docker ps
docker logs -f cicd-nginx

# Restart services
docker restart cicd-jenkins
docker restart cicd-nginx

# Update containers
docker-compose pull && docker-compose up -d
Troubleshooting:
bash
# Check if Nginx is accessible locally on Arch
curl http://localhost:8080

# Check container networks
docker network ls
docker inspect cicd-nginx

# Verify port mapping
ss -tlnp | grep 8080
🚨 TROUBLESHOOTING
If Jenkins is inaccessible:
Check containers are running on Arch:

bash
ssh archserver docker ps
Test locally on Arch server:

bash
curl http://localhost:8080
Check Nginx configuration:

bash
ssh archserver docker logs cicd-nginx
Verify firewall on Arch:

bash
sudo ufw status
If Windows can't resolve hostname:
Use IP directly: http://192.168.0.130:8080

Or add archserver to Windows hosts file

📋 QUICK START COMMANDS
From Windows CMD/PowerShell:

cmd
# Test connectivity
ping 192.168.0.130

# Access Jenkins
start http://archserver:8080
From WSL:

bash
# SSH to Arch for management
ssh archserver

# Or with tunnel (if direct access fails)
ssh -L 8080:localhost:8080 archserver
Recommendation: Use Method 1 (Direct Nginx Access) for daily use since it's already properly configured with Nginx reverse proxy. Use SSH tunnels only if you need additional security or if direct access isn't working.

