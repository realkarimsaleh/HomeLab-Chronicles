# Debranding a Linksys SPNMX42

This guide outlines two approaches for debranding a single Community Fibre-provided Linksys MX4200 (also known as SPNMX42 or AX4200) router. Debranding allows for enhanced control, unlocking additional settings, firmware update options, and improved compatibility with non-ISP configurations.

---

## Important Notes

- **Backup Settings**: Back up any critical router settings, as firmware changes may affect existing configurations.
- **Proceed with Caution**: Debranding involves firmware updates, which can impact router functionality. Confirm compatibility before proceeding.
- **Tools Needed**: Ensure you have access to a computer connected to the router’s network and, if possible, a Linksys app to monitor IP changes.

---

## Method 1: Using the StarHub and Retail Firmware Swap (Recommended for Simplicity)

This approach uses firmware versions from StarHub and Linksys Retail, allowing for smoother updates while maintaining control over the router’s settings.

### Step-by-Step Instructions

1. **Access the Router’s Web Interface**
   - Connect to the router's network and open a browser.
   - Enter `192.168.1.1` to access the router’s web interface.
   - Log in with your admin credentials.

2. **Upload StarHub Firmware**
   - Navigate to the **Connectivity** section, and select the **CA** option at the bottom of the page.
   - In the **Manual Firmware Update** section, choose the StarHub firmware file (`FW_MX42SH_1.0.10.210447_prod.img`).
   - Click "Upload" and wait for the firmware to install. The router may restart and change its IP address to `192.168.10.1`.
   - Log in again using the new IP address.

3. **Upload Linksys Retail Firmware**
   - Return to the **CA** section and select **Choose File** in the **Manual Firmware Update** area.
   - Upload the Retail firmware (`FW_MX4200_1.0.13.210200_prod.img`) and click "Upload."
   - Allow the firmware to install and for the router to reboot.

4. **Confirm Debranding Success**
   - Log back in to the router’s web interface using the new IP (`192.168.1.1` or `192.168.10.1`).
   - Check the firmware version in the **Settings** > **Firmware** section to ensure the Retail firmware has been applied.

This method successfully debrands the router, providing control over firmware updates and unlocking additional features.

---

## Method 2: Using OpenWRT and Then Upgrading to Retail Firmware

This approach involves first installing OpenWRT, which enables a wider range of customization options, and then transitioning to Linksys Retail firmware for standard functionality.

### Step-by-Step Instructions

1. **Access the Web Interface**
   - Connect to the router's network, open a web browser, and go to `192.168.1.1`.
   - Log in using admin credentials.

2. **Install OpenWRT Firmware**
   - Go to the **CA** section under **Connectivity**.
   - In **Manual Firmware Update**, select the OpenWRT firmware file (`openwrt-ramips-mt7621-xxxx-squashfs-sysupgrade.bin`).
   - Click "Upload" and allow the firmware to install. The router may reboot and change its IP address to `192.168.1.1` (default for OpenWRT).

3. **Log into OpenWRT**
   - After reboot, log in to the OpenWRT web interface at `192.168.1.1`.
   - Configure basic settings, such as network setup, if necessary.

4. **Transition to Linksys Retail Firmware (via SSH)**

   - **Access the Router via SSH**
     - Connect your computer to the router’s network.
     - Open a terminal (or SSH client like PuTTY) and enter the following command to access the router:
       ```bash
       ssh root@192.168.1.1
       ```
     - Enter the password if prompted (default is usually empty or whatever you set during OpenWRT setup).

   - **Upload the Linksys Retail Firmware to the Router**
     - Open a new terminal on your computer and use `scp` (or an equivalent file transfer command) to upload the Retail firmware to the router:
       ```bash
       scp /path/to/FW_MX4200_1.0.13.210200_prod.img root@192.168.1.1:/tmp
       ```
     - Replace `/path/to/` with the actual path to the firmware file on your computer.

   - **Flash the Linksys Retail Firmware**
     - In the SSH session, navigate to the directory where you uploaded the firmware:
       ```bash
       cd /tmp
       ```
     - Verify the file’s integrity by running a checksum (optional but recommended):
       ```bash
       md5sum FW_MX4200_1.0.13.210200_prod.img
       ```
     - Flash the firmware using the `mtd` command:
       ```bash
       mtd -r write FW_MX4200_1.0.13.210200_prod.img firmware
       ```
     - The `-r` option automatically reboots the router after the firmware is written.

5. **Confirm the Firmware Transition**
   - After the router reboots, access the router’s web interface at `192.168.1.1` or `192.168.10.1`.
   - Log in to confirm that the Retail firmware is active and verify the firmware version in **Settings** > **Firmware**.

This SSH-based method transitions your router from OpenWRT to Linksys Retail firmware without requiring the OpenWRT web interface, providing a command-line approach to debranding.

---

## Troubleshooting and Tips

- **Redirects to Parent Node Interface**: If redirected to another node’s interface, give it a few minutes and retry.
- **Connection Issues Post-Firmware Update**: Reset the router to factory settings and reconfigure it manually.
- **Using the Linksys App**: For IP changes or device discovery, the Linksys app can show the updated IP address of your router.

---

## Additional Resources

- [ISPreview Guide on Debranding Linksys MX4200](https://www.ispreview.co.uk/talk/threads/how-to-debrand-a-linksys-mx4200-v1.39281/): Detailed user discussions on debranding the MX4200.
- [Community Fibre WHW03CFv2 Guide on GitHub](https://github.com/ishi0/Community-Fibre-WHW03CFv2/wiki): Guide by [@ishi0](https://github.com/ishi0) with additional details on firmware flashing and troubleshooting.

By following either of these methods, you can successfully debrand your Community Fibre router and unlock additional configuration options.

