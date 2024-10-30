# Configuring macOS Hostname via Command Line Interface (CLI)

This article provides instructions for setting the primary hostname, Bonjour hostname, and computer name on macOS through the Command Line Interface (CLI).

---

### Step 1: Access Terminal

1. Launch the Terminal application by navigating to `Applications > Utilities > Terminal` or by using Spotlight (`Cmd + Space`) to search for "Terminal."

---

### Step 2: Set the Primary Hostname

The primary hostname, also known as the Fully Qualified Domain Name (FQDN), uniquely identifies your Mac on external networks.

1. Enter the following command in Terminal:
   ```bash
   sudo scutil --set HostName <new host name>
   ```
   Replace `<new host name>` with your desired hostname in the format `hostname.domain.com`.
   
   **Example**:
   ```bash
   sudo scutil --set HostName hostname.domain.com
   ```
2. You will be prompted to enter your password to authorize this change.

---

### Step 3: Set the Bonjour (Local) Hostname

The Bonjour hostname (or Local Hostname) is visible on local networks, typically ending with `.local`.

1. To configure the Bonjour hostname, use:
   ```bash
   sudo scutil --set LocalHostName <new host name>
   ```
   Replace `<new host name>` with your chosen name (e.g., `hostname`).
   
   **Example**:
   ```bash
   sudo scutil --set LocalHostName hostname
   ```

---

### Step 4: Set the Computer Name

The Computer Name is the user-friendly name visible in Finder and in other macOS services.

1. Set the Computer Name by running:
   ```bash
   sudo scutil --set ComputerName <new name>
   ```
   Replace `<new name>` with your preferred name for display purposes (e.g., `hostname`).
   
   **Example**:
   ```bash
   sudo scutil --set ComputerName hostname
   ```

---

### Step 5: Flush the DNS Cache

Flush the DNS cache to ensure the new hostname settings are recognized:

```bash
dscacheutil -flushcache
```

---

### Step 6: Restart macOS

Restart your Mac to apply all hostname changes across system services.

---

### Hostname Types and Their Uses

- **Primary Hostname (FQDN)**: Used to identify your Mac on broader networks.
- **Bonjour Hostname**: Appears on the local network, usually formatted as `.local`.
- **Computer Name**: Visible in Finder and other macOS interfaces for easy identification.
