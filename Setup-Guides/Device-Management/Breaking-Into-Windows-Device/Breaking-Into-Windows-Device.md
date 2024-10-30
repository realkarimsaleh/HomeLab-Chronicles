# Breaking into Windows System

This guide explains how to temporarily replace **Utilman.exe** with **Cmd.exe** to gain access to a Windows system. This method is useful if you’re locked out of the system or need to reset a local administrator password. By using this approach, you can access the system with elevated privileges and reset the **Administrator** account or create a new one.

> **Note**: This method should be used with permission and strictly for troubleshooting or recovery purposes, as it involves modifying critical system files.

---

## Prerequisites

- **Windows Installation Media**: You’ll need a USB or DVD with the Windows installation files to boot into recovery mode.
- **Basic Command Prompt Knowledge**: The instructions involve entering commands in a Command Prompt interface.

---

## Steps to Access and Reset the Administrator Account

### Step 1: Boot into Windows Recovery Environment (WinRE)
1. Insert the Windows installation media (USB or DVD) and restart the computer.
2. As the computer boots, press the required key to access the **Boot Menu** (often `F12`, `F2`, or `ESC` depending on your computer’s brand).
3. Select the installation media (USB or DVD) to boot from.
4. When the **Windows Setup** screen appears, choose your language and click **Next**.
5. Click on **Repair your computer** at the bottom left of the screen.
6. Select **Troubleshoot** > **Advanced options** > **Command Prompt**.

   This opens a Command Prompt window that allows you to make changes to system files.

### Step 2: Backup the Original Utilman.exe and Replace It with Cmd.exe
1. In the Command Prompt window, type the following command to create a backup of the **Utilman.exe** file (this is the Accessibility/Ease of Access program):

   ```cmd
   copy c:\windows\system32\utilman.exe c:\windows\system32\utilman.exe.old
   ```

2. Now, replace **Utilman.exe** with **Cmd.exe** by typing:

   ```cmd
   copy c:\windows\system32\cmd.exe c:\windows\system32\utilman.exe
   ```

   - You may be prompted to confirm the replacement. If so, type `Y` and press **Enter** to confirm.

3. Once done, restart the computer by entering the following command:

   ```cmd
   wpeutil reboot
   ```

   The computer will now reboot normally.

### Step 3: Access the Command Prompt from the Login Screen
1. When the system reboots, reach the Windows login screen.
2. Click on the **Accessibility** (Ease of Access) icon in the bottom-right corner of the screen. This icon typically resembles a person with arms extended.
3. Instead of opening the usual Accessibility menu, a Command Prompt window will open with administrative privileges.

### Step 4: Enable and Reset the Administrator Account
1. In the Command Prompt, type the following command to enable the built-in **Administrator** account if it is disabled:

   ```cmd
   net user Administrator /active:yes
   ```

2. To set a new password for the Administrator account, enter:

   ```cmd
   net user Administrator NewPassword
   ```

   - Replace `NewPassword` with a secure password of your choice. Make sure to remember this password, as it will be needed to log in.

   Example:

   ```cmd
   net user Administrator MyNewP@ssw0rd!
   ```

3. Close the Command Prompt window and log in to Windows using the **Administrator** account with the new password.

### Step 5: Restore the Original Utilman.exe File
After you have successfully accessed the system and completed the necessary tasks, it’s important to restore **Utilman.exe** to its original state.

1. Reboot the computer back into the Windows Recovery Environment using the installation media (following **Step 1**).
2. Open the **Command Prompt** once again through **Troubleshoot** > **Advanced options** > **Command Prompt**.
3. Enter the following command to restore the original **Utilman.exe** file:

   ```cmd
   copy c:\windows\system32\utilman.exe.old c:\windows\system32\utilman.exe
   ```

4. Confirm the replacement if prompted by typing `Y` and pressing **Enter**.
5. Restart the computer by typing:

   ```cmd
   wpeutil reboot
   ```

---

## Important Notes

- **Use with Caution**: This method provides administrative access and should only be used by authorized personnel.
- **Disable the Administrator Account (Optional)**: If you re-enabled the **Administrator** account, consider disabling it again for security purposes by opening a Command Prompt and entering:
  
   ```cmd
   net user Administrator /active:no
   ```

By following this guide, you can temporarily gain access to a locked Windows system using a command-line approach, allowing you to reset or enable the Administrator account.
