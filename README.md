# Guide to Running an Android Emulator and Screen Mirroring a Physical Device `was written by Pham Minh Thao (Trunks-Pham)`

## 1. Running an Android Emulator Without Android Studio

### Requirements
- **Operating System**: Windows, macOS, or Linux.
- **System Specs**: CPU with virtualization support (Intel VT-x or AMD-V), minimum 8GB RAM (16GB recommended).
- **Android SDK Tools**: Provides emulator and necessary tools.
- **HAXM (Intel) or Hypervisor Framework (AMD/macOS)**: For emulator acceleration.

### Steps to Install and Run Emulator
1. **Download Android SDK Command-Line Tools**:
   - Visit: <https://developer.android.com/studio#command-line-tools>
   - Download `commandlinetools-win-...zip` (Windows) or the appropriate version for macOS/Linux.
   - Extract to a folder, e.g., `C:\Android\cmdline-tools`.
   - Create a subfolder `latest` and move extracted contents to `C:\Android\cmdline-tools\latest`.
2. **Install SDK and Emulator**:
   - Open Command Prompt (Windows) or terminal (macOS/Linux).
   - Add `C:\Android\cmdline-tools\latest\bin` to the system **Path**:
     - Windows: **System Properties > Environment Variables > Path > New > C:\Android\cmdline-tools\latest\bin**.
   - Run:
     ```
     sdkmanager "platform-tools" "emulator" "platforms;android-33" "system-images;android-33;google_apis;x86_64"
     ```
   - Accept licenses when prompted (`y`).
3. **Install HAXM (Intel) or Hypervisor Framework (AMD/macOS)**:
   - **Windows (Intel)**: Download HAXM from <https://github.com/intel/haxm/releases> and install.
   - **AMD/macOS**: Ensure Hypervisor Framework is enabled (usually automatic).
4. **Create AVD (Android Virtual Device)**:
   - Run:
     ```
     avdmanager create avd -n thaopm -k "system-images;android-33;google_apis;x86_64" -d pixel_6
     ```
     - Replace `thaopm` with your desired AVD name.
     - Select a device (e.g., `pixel_6`) when prompted.
5. **Run Emulator**:
   - List AVDs:
     ```
     emulator -list-avds
     ```
   - Run AVD:
     ```
     emulator -avd thaopm
     ```
   - The emulator will start, showing the virtual device interface.

### Notes
- **Performance**: Enable virtualization in BIOS (Intel VT-x/AMD-V) for smoother performance.
- **Common Issues**:
  - If the emulator fails to start, check HAXM/Hypervisor Framework or update GPU drivers.
  - Ensure `emulator` is in **Path** (typically at `C:\Android\emulator`).

---

## 2. Enable USB Debugging on a Physical Device

### Steps
1. **Enable Developer Options**:
   - Go to **Settings > About phone**.
   - Tap **Build Number** 7 times to unlock **Developer Options**.
2. **Enable USB Debugging**:
   - Go to **Settings > Developer Options**, enable **USB Debugging**.
3. **Grant Permission**:
   - Connect the phone to the PC via USB.
   - When prompted with **Allow USB debugging**, select **Allow** (optionally, **Always allow**).

### Notes
- If no prompt appears, check USB mode (select **File Transfer (MTP)** or **PTP** in the notification bar).
- Use a USB cable that supports data transfer.

---

## 3. Using ADB and scrcpy for Screen Mirroring

### Requirements
- **ADB**: Tool for interacting with Android devices.
- **scrcpy**: Software for screen mirroring.
- **Android Device**: Android 5.0 or higher, with USB Debugging enabled.
- **PC**: Windows, macOS, or Linux.

### Install ADB
1. **Download ADB**:
   - Download from: <https://dl.google.com/android/repository/platform-tools-latest-windows.zip>
   - Extract to `C:\Program Files\platform-tools`.
2. **Add to Path**:
   - Windows: **System Properties > Environment Variables > Path > New > C:\Program Files\platform-tools**.
3. **Verify**:
   - Run:
     ```
     adb --version
     ```
   - Expected output: `Android Debug Bridge version 1.0.41`.

### Install scrcpy
1. **Download scrcpy**:
   - Download from: <https://github.com/Genymobile/scrcpy/releases>
   - Choose the `.zip` file (e.g., `scrcpy-win64-v3.3.zip`).
   - Extract to `C:\Program Files\scrcpy-win64-v3.3`.
2. **Add to Path**:
   - Add `C:\Program Files\scrcpy-win64-v3.3` to **Path**.
3. **Verify**:
   - Run:
     ```
     scrcpy --version
     ```
   - Expected output: `scrcpy 3.3`.

### Using ADB and scrcpy
1. **Connect Device**:
   - Connect via USB, enable **USB Debugging**.
   - Run:
     ```
     adb devices
     ```
   - If a serial number with `device` status appears (e.g., `1234567890abc device`), the connection is successful.
2. **Run scrcpy**:
   - Run:
     ```
     scrcpy
     ```
   - The phone’s screen will appear on the PC.
3. **Customization (Optional)**:
   - Limit resolution: `scrcpy --max-size 1920`.
   - Record screen: `scrcpy --record file.mp4`.
   - More options: `scrcpy --help`.

### Troubleshooting
- **ADB doesn’t detect device**:
  - Check USB cable (must support data transfer).
  - Try a different USB port.
  - Install USB drivers from the phone manufacturer or Universal ADB Driver: <https://adb.clockworkmod.com/>
  - Run:
    ```
    adb kill-server
    adb start-server
    adb devices
    ```
- **scrcpy error "Could not find any ADB device"**:
  - Ensure `adb devices` lists the device.
  - Install libusb: <https://libusb.info/>
- **scrcpy lag**:
  - Reduce resolution: `scrcpy --max-size 1280`.
  - Reduce bitrate: `scrcpy --bit-rate 2M`.

---

## 4. Connect via Wi-Fi (Optional)
1. Connect via USB, run:
   ```
   adb tcpip 5555
   ```
2. Disconnect USB, find the phone’s IP (go to **Settings > Wi-Fi**).
3. Connect:
   ```
   adb connect <phone_IP>:5555
   ```
4. Run `scrcpy`.

---

## 5. Notes
- **Emulator**: Enable virtualization in BIOS and install HAXM/Hypervisor Framework.
- **scrcpy**: Streams video only, not audio. Supports Android only.
- **Updates**: Use the latest versions of ADB and scrcpy.
- **Detailed errors**: Run `scrcpy -v` for debug logs.

---

## 6. Contact for Work
- **GitHub** ([Trunks-Pham](https://github.com/Trunks-Pham))
- **LinkedIn** ([Pham Minh Thao](https://www.linkedin.com/in/mtpe-minhthaopham))
- **Email** ([Pham Minh Thao](mailto:minhthaopham230104@gmail.com))

---

## 7. Copyright
© 2025 Pham Minh Thao (Trunks-Pham). All rights reserved.  
Version: 2025