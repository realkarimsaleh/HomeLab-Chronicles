# How to Enable Hyper-V on Windows

Hyper-V is a virtualization technology from Microsoft that enables you to create and run virtual machines on Windows. This article provides step-by-step instructions for enabling Hyper-V on Windows.

## Prerequisites

To enable Hyper-V, your system must meet the following requirements:
- **Windows Version**: Windows 10 Pro, Enterprise, or Education editions
- **Processor**: 64-bit processor with Second Level Address Translation (SLAT)
- **RAM**: Minimum of 4GB

## Steps to Enable Hyper-V

### 1. Verify System Requirements

Before enabling Hyper-V, ensure your system meets the requirements. To check your system details:
1. Press `Windows + R` to open the Run dialog.
2. Type `msinfo32` and press `Enter`.
3. In the System Information window, confirm the following under **Processor**:
   - **Virtualization Enabled in Firmware**: Yes
   - **Second Level Address Translation**: Available

### 2. Enable Hyper-V Using Windows Features

#### Option A: Enable via Control Panel

1. **Open Control Panel**: Press `Windows + S` and search for **Control Panel**.
2. **Navigate to Programs**: Select **Programs** > **Turn Windows features on or off**.
3. **Enable Hyper-V**:
   - In the Windows Features dialog, check the box next to **Hyper-V**.
   - Expand **Hyper-V** to ensure both **Hyper-V Management Tools** and **Hyper-V Platform** are selected.
4. **Apply and Restart**: Click **OK** to apply the changes, then restart your computer to complete the installation.

#### Option B: Enable Hyper-V Using PowerShell

1. **Open PowerShell**:
   - Press `Windows + X` and select **Windows PowerShell (Admin)** to open PowerShell as Administrator.
2. **Run the Enable Command**:
   - Enter the following command to enable Hyper-V:
     ```powershell
     Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
     ```
   - If the command isn't found, ensure you're running PowerShell as Administrator.
3. **Restart the Computer**: After the command completes, restart your computer to apply the changes.

### 3. Verify Hyper-V Installation

After enabling Hyper-V, verify that it’s installed and available:

1. **Open Hyper-V Manager**:
   - Press `Windows + S` and search for **Hyper-V Manager**.
   - Select **Hyper-V Manager** from the search results.
2. **Confirm Installation**:
   - If Hyper-V Manager opens without errors, Hyper-V is successfully enabled on your computer.

## Troubleshooting

If you encounter issues enabling Hyper-V, try the following troubleshooting steps:

- **Check BIOS Settings**:
  - Ensure virtualization is enabled in your BIOS/UEFI settings. Restart your computer, enter the BIOS/UEFI menu (usually by pressing `Delete`, `F2`, or `Esc` during startup), and enable **Intel VT-x** or **AMD-V** under CPU or Advanced settings.
  
- **Windows Version Compatibility**:
  - Hyper-V is only available on Windows 10 Pro, Enterprise, and Education editions. If you’re using Windows 10 Home, consider upgrading your Windows version.

## Additional Resources

[Microsoft Hyper-V Documentation](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

---

With Hyper-V enabled, you can start creating and managing virtual machines on your Windows system. For guides on creating a virtual machine, see our [Hyper-V VM Setup Guide](link-to-your-vm-setup-guide).
