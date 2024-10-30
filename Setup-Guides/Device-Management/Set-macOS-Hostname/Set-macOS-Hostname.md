# Set macOS Hostname: Instructions for Using the Command Line Interface (CLI)

This guide provides step-by-step instructions to configure the primary hostname, Bonjour hostname, and computer name on macOS.

---

### Step 1: Open Terminal

1. Open your Terminal application to get started. You can find it in `Applications > Utilities > Terminal` or by searching for "Terminal" using Spotlight (`Cmd + Space`).

### Step 2: Change the Primary Hostname

The primary hostname is your Mac's fully qualified domain name (FQDN), commonly in the format `MacName.domain.com`. To set this:

1. Type the following command:
   ```bash
   sudo scutil --set HostName <new host name>
   ```
   Replace `<new host name>` with your desired hostname, like `hostname.domain.co.uk`.
   
   **Example**:
   ```bash
   sudo scutil --set HostName hostname.domain.co.uk
   ```
2. Enter your password when prompted to complete this action.

### Step 3: Change the Bonjour Hostname

The Bonjour hostname, or Local Hostname, is visible on the local network and usually ends with `.local`. Setting this name helps others on your network recognize your Mac.

1. Use this command:
   ```bash
   sudo scutil --set LocalHostName <new host name>
   ```
   Replace `<new host name>` with your chosen name, such as `hostname`.
   
   **Example**:
   ```bash
   sudo scutil --set LocalHostName hostname
   ```

### Step 4: Set the Computer Name

The Computer Name is the user-friendly name visible in Finder and various other services. To set this:

1. Enter this command:
   ```bash
   sudo scutil --set ComputerName <new name>
   ```
   Replace `<new name>` with your preferred computer name, like `hostname`.
   
   **Example**:
   ```bash
   sudo scutil --set ComputerName hostname
   ```

### Step 5: Flush the DNS Cache

To ensure your Mac recognizes these changes, flush the DNS cache by typing:
```bash
dscacheutil -flushcache
```

### Step 6: Restart Your Mac

Finally, restart your Mac to apply the new hostname settings across all services.

---

**Note:** Each hostname is used in different contexts:
- **Primary Hostname (FQDN):** Used for identifying your Mac on wide networks.
- **Bonjour Hostname:** Appears on the local network, usually ending in `.local`.
- **Computer Name:** Displays in Finder and is used for personal identification in macOS services.
