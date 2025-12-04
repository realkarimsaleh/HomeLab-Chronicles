# DDNS on a Raspberry Pi using Cloudflare API

This guide provides step-by-step instructions for setting up Dynamic DNS (DDNS) on any Unix based machine with Cloudflare's API. DDNS keeps your domain pointed at your home network even when your ISP assigns a dynamic public IP address that changes frequently.

---

## Important Notes

- **Prerequisites**: Linux system (can be a VM) with internet access, a Cloudflare account with a domain, and some basic terminal familiarity.
- **Proceed with Caution**: API tokens grant DNS control; store them securely and limit permissions to the specific zone.
- **Tools Needed**: SSH access to your Linux Host, my Prefered SSH Client is [Termius](https://termius.com/index.html)

---

## What is DDNS and Why Do You Need It?

If you are on a residential connection from your ISP, your public IP address may change frequently. DDNS solves this by automatically updating a DNS record (like a subdomain) with your current public IP, ensuring services like remote access or home servers remain reachable via a consistent domain name.

---

## Step-by-Step Instructions

### Step 1: Cloudflare Setup

1. **Log into Cloudflare Dashboard**
   - Sign into your Cloudflare account and select your domain (zone).
   - Go to **DNS** > **Records** > **Add Record**.
   - Create an **A record** (e.g., `home.example.com`) pointing to any IP (it will update dynamically). Set proxy to **DNS only** for direct IP access.

2. **Create API Token**
   - Navigate to **My Profile** > **API Tokens** > **Create Token**.
   - Use the **Edit zone DNS** template: Select your zone, **Zone: DNS:Edit** permissions.
   - Copy the generated **API Token** (recommended for security).  
     **Alternative:** Use your **Global API Key** from **API Tokens** > **API Keys** (less secure, broader permissions).

3. **Get Zone and Record IDs**
   - Note your **Zone ID** from the right sidebar in the dashboard.

### Step 2: Install Cloudflare DDNS Updater Script

1. **Access Your Linux Host**
   - SSH into your Linux system.

2. **Clone the Repository**
   - Run:  
     ```
     Sudo Bash
     cd
     git clone https://github.com/K0p1-Git/cloudflare-ddns-updater.git
     cd cloudflare-ddns-updater
     ```

3. **Configure the Script**
   - Copy the config example:  
     ```
     cp cloudflare-template.sh cloudflare.sh
     nano cloudflare.sh
     ```
   - Edit the config file with your values:
     - `auth_email="Email you use to login to Cloudflare with"`
     - `auth_method="Set to "global" for Global API Key or "token" for Scoped API Token"` 
     - `auth_key="Your API Token or Global API Key"` 
     - `zone_id="Your Zone ID"`
     - `record_name=="Your A Record"`

4. **Make Executable and Test**
   - Run:  
     ```
     chmod +x cloudflare.sh
     ./cloudflare.sh
     ```
   - Verify in Cloudflare DNS dashboard that the A record IP matches your public IP.


### Step 3: Automate with Crontab

1. **Edit Crontab**
   - Run:  
     ```
     crontab -e
     ```
   - Select nano if prompted.

2. **Add Cron Job**
   - Add the following line to run the script every 1 minutes:
     ```
     */1 * * * * /root/cloudflare-ddns-updater/cloudflare.sh
     ```

3. **Restart Cron**
   - Run:  
     ```
     systemctl restart cron
     ```

3. **Verify**
   - Check cron logs or monitor DNS updates as needed.
   - Run:  
     ```
     systemctl status cron
     ```

---

## Additional Resources

- [NetworkChuck's YouTube Video](https://www.youtube.com/watch?v=rI-XxnyWFnM): Full visual walkthrough and explanation.
- [Cloudflare DDNS Script Repo](https://github.com/K0p1-Git/cloudflare-ddns-updater/tree/main):  Script developed by [@K0p1-Git](https://github.com/K0p1-Git). 

By following these steps, you will automatically keep your Cloudflare DNS record in sync with your dynamic public IP for seamless remote access.
