# Exposing-Localhost-with-Cloudflare-Tunnel-No-Domain-Required-

This guide explains how to expose a local server running on your Ubuntu system to the internet using Cloudflare Tunnel without requiring a custom domain.

---

## Prerequisites
- **Ubuntu system**
- Local server running (e.g., Python HTTP server, Apache, or Nginx)
- Internet connection

---

## Steps to Set Up Cloudflare Tunnel

### Step 1: Install Cloudflare Tunnel (cloudflared)
1. Download the cloudflared binary:
   ```bash
   wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
   ```
2. Install the package:
   ```bash
   sudo dpkg -i cloudflared-linux-amd64.deb
   ```
3. Verify the installation:
   ```bash
   cloudflared --version
   ```

---

### Step 2: Run Your Local Server
Start a local server on your machine. For example, using Python:
```bash
python3 -m http.server 8080
```
Ensure the server is running and accessible at `http://localhost:8080`.

---

### Step 3: Start Cloudflare Tunnel
Run the following command to create a tunnel and expose your localhost:
```bash
cloudflared tunnel --url http://localhost:8080
```

Cloudflare will generate a public URL that looks like this:
```
https://randomname.trycloudflare.com
```

You can share this URL to provide external access to your local server.

---

### Step 4: Optional - Running Cloudflare Tunnel as a Service
To make the tunnel persist even after a reboot:

1. Install the cloudflared service:
   ```bash
   sudo cloudflared service install
   ```
2. Configure the service by editing the configuration file:
   ```bash
   sudo nano /etc/cloudflared/config.yml
   ```
   Add the following lines:
   ```yaml
   url: http://localhost:8080
   no-autoupdate: true
   ```
3. Start and enable the service:
   ```bash
   sudo systemctl start cloudflared
   sudo systemctl enable cloudflared
   ```

---

## Troubleshooting
- Ensure your local server is running on the specified port.
- Check the logs for any errors:
  ```bash
  sudo journalctl -u cloudflared.service
  ```
- Make sure your firewall allows outbound connections to Cloudflare.

---

## References
- [Cloudflare Tunnel Documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)
- [TryCloudflare GitHub Repository](https://github.com/cloudflare/cloudflared)

---

Enjoy sharing your localhost securely with Cloudflare Tunnel!
