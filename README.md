# Bypass MDM Enhanced (rponeawa)

[Chinese Version / 中文版](README-CN.md)

This project extends the original MDM bypass script by Assaf Dori. This enhanced version incorporates core bypass and persistence logic derived from the reverse engineering of **micaixin.cn** and analysis of scripts from **多啦快解 (Dora Fast Solve)** on Xianyu.

---

## Technical Enhancements

This version implements the following technical features identified through binary and script analysis:

### 1. From micaixin.cn Analysis
*   **System Daemon Suppression**: Initializes the system flag `/var/db/.com.apple.mdmclient.daemon.forced_disable` to force the `mdmclient` process to terminate upon start.
*   **Direct Configuration Modification**: Uses `PlistBuddy` to explicitly set `CloudConfigRecordFound`, `CloudConfigHasActivationRecord`, and `CloudConfigProfileInstalled` to `false` in the core system database.
*   **Hardware-level Attribute Locking**: Applies the `uchg` (User Immutable) flag to all bypass markers and Plist configurations to prevent automated system restoration.
*   **IPv6 Connectivity Blocking**: Includes IPv6 (`::`) entries in the hosts file to prevent MDM synchronization via IPv6 tunnels.

### 2. From 多啦快解 (Dora Fast Solve) Analysis
*   **FileVault Disk Decryption**: Includes logic to detect and unlock APFS volumes protected by FileVault, ensuring accessibility to the system database.
*   **Extended Service Suppression**: Implements explicit `launchctl` disable commands for `cloudconfigurationd` and other management agents as an additional layer of defense.

---

## Installation and Usage

Follow these procedures to bypass MDM enrollment during a fresh macOS installation:

**1. Shutdown**
Perform a hard shutdown of the Mac.

**2. Boot into Recovery Mode**
*   Apple Silicon: Hold the Power button until Startup Options appear.
*   Intel: Hold Command + R during the boot sequence.

**3. Network Activation**
Connect to a Wi-Fi network to ensure the Mac is activated.

**4. Terminal Initialization**
Select Utilities from the menu bar and open Terminal.

**5. Execution**
Run the following command:
```bash
curl -L https://raw.githubusercontent.com/rponeawa/bypass-mdm-enhanced/main/bypass-mdm-enhanced.sh -o bypass-mdm.sh && chmod +x ./bypass-mdm.sh && ./bypass-mdm.sh
```

**6. Bypass Selection**
Select Option 1: "Bypass MDM from Recovery".

**7. Account Configuration**
Configure the temporary administrator account or utilize default values.

**8. Finalization**
Wait for the confirmation message: "Bypass Completed Successfully".

**9. Reboot**
Exit the Terminal and restart the Mac.

---

## Post-Installation Steps

**10. Authentication**
Login using the temporary account (Default: Apple / 1234).

**11. Setup Assistant**
Skip all introductory prompts (Apple ID, Siri, Touch ID, Location Services).

**12. Primary Account Creation**
Navigate to System Settings > Users and Groups and create a permanent administrator account.

**13. System Cleanup**
Delete the temporary administrator account from System Settings.

---

## Troubleshooting

### Volume Detection Failure
Verify the device is in Recovery Mode and that a valid macOS installation exists on the target drive.

### Permission Denied
Ensure the script has execution permissions: `chmod +x bypass-mdm.sh`.

---

**Disclaimer**: This tool is for educational and research purposes only.
