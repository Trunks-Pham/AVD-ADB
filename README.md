# Hướng Dẫn Chạy Điện Thoại Ảo Độc Lập và Chia Sẻ Màn Hình Điện Thoại Thật

## 1. Chạy Điện Thoại Ảo (Emulator) Không Qua Android Studio

### Yêu cầu
- **Hệ điều hành**: Windows, macOS, hoặc Linux.
- **Cấu hình máy**: CPU hỗ trợ ảo hóa (Intel VT-x hoặc AMD-V), RAM tối thiểu 8GB (khuyến nghị 16GB).
- **Android SDK Tools**: Cung cấp emulator và công cụ cần thiết.
- **HAXM (Intel) hoặc Hypervisor Framework (AMD/macOS)**: Tăng tốc emulator.

### Các bước cài đặt và chạy emulator độc lập
1. **Tải Android SDK Command-Line Tools**:
   - Truy cập: <https://developer.android.com/studio#command-line-tools>
   - Tải file `commandlinetools-win-...zip` (Windows) hoặc phiên bản phù hợp với macOS/Linux.
   - Giải nén vào thư mục, ví dụ: `C:\Android\cmdline-tools`.
   - Tạo thư mục con `latest` và di chuyển nội dung giải nén vào `C:\Android\cmdline-tools\latest`.
2. **Cài đặt SDK và Emulator**:
   - Mở Command Prompt (Windows) hoặc terminal (macOS/Linux).
   - Thêm `C:\Android\cmdline-tools\latest\bin` vào biến môi trường **Path**:
     - Windows: **System Properties > Environment Variables > Path > New > C:\Android\cmdline-tools\latest\bin**.
   - Chạy lệnh để cài đặt emulator và system image:
     ```
     sdkmanager "platform-tools" "emulator" "platforms;android-33" "system-images;android-33;google_apis;x86_64"
     ```
   - Chấp nhận giấy phép khi được hỏi (`y`).
3. **Cài đặt HAXM (Intel) hoặc Hypervisor Framework (AMD/macOS)**:
   - **Windows (Intel)**: Tải HAXM từ <https://github.com/intel/haxm/releases>, cài đặt để tăng tốc emulator.
   - **AMD/macOS**: Đảm bảo Hypervisor Framework được bật (thường tự động trên macOS hoặc Windows với Hyper-V).
4. **Tạo AVD (Android Virtual Device)**:
   - Chạy:
     ```
     avdmanager create avd -n thaopm -k "system-images;android-33;google_apis;x86_64" -d pixel_6
     ```
     - Thay `thaopm` bằng tên AVD bạn muốn.
     - Chọn thiết bị (ví dụ: `pixel_6`) khi được hỏi.
5. **Chạy Emulator**:
   - Liệt kê AVD:
     ```
     emulator -list-avds
     ```
   - Chạy AVD:
     ```
     emulator -avd thaopm
     ```
   - Emulator sẽ khởi động, hiển thị giao diện điện thoại ảo.

### Lưu ý
- **Hiệu suất**: Bật ảo hóa trong BIOS (Intel VT-x/AMD-V) để emulator chạy mượt.
- **Lỗi thường gặp**:
  - Nếu emulator không khởi động, kiểm tra HAXM/Hypervisor Framework hoặc cập nhật driver GPU.
  - Đảm bảo `emulator` được thêm vào **Path** (thường tại `C:\Android\emulator`).

---

## 2. Bật Gỡ Lỗi Qua USB Trên Điện Thoại Thật

### Các bước
1. **Kích hoạt chế độ nhà phát triển**:
   - Vào **Cài đặt > Giới thiệu về điện thoại**.
   - Nhấn liên tục vào **Số build** (7 lần) để mở **Tùy chọn nhà phát triển**.
2. **Bật Gỡ lỗi USB**:
   - Vào **Cài đặt > Tùy chọn nhà phát triển**, bật **Gỡ lỗi USB (USB Debugging)**.
3. **Cấp quyền khi kết nối**:
   - Kết nối điện thoại với PC qua USB.
   - Khi thông báo **Cho phép gỡ lỗi USB** xuất hiện, chọn **Cho phép** (có thể chọn **Luôn cho phép**).

### Lưu ý
- Nếu không thấy thông báo, kiểm tra chế độ USB (chọn **Chuyển tệp (MTP)** hoặc **PTP** trong thanh thông báo).
- Dùng cáp USB hỗ trợ truyền dữ liệu.

---

## 3. Sử Dụng ADB và scrcpy để Chia Sẻ Màn Hình Điện Thoại Thật

### Yêu cầu
- **ADB**: Công cụ giao tiếp với thiết bị Android.
- **scrcpy**: Phần mềm stream màn hình.
- **Điện thoại Android**: Android 5.0 trở lên, đã bật Gỡ lỗi USB.
- **PC**: Windows, macOS, hoặc Linux.

### Cài đặt ADB
1. **Tải ADB**:
   - Tải từ: <https://dl.google.com/android/repository/platform-tools-latest-windows.zip>
   - Giải nén vào `C:\Program Files\platform-tools`.
2. **Thêm vào Path**:
   - Windows: **System Properties > Environment Variables > Path > New > C:\Program Files\platform-tools**.
3. **Kiểm tra**:
   - Chạy:
     ```
     adb --version
     ```
   - Kết quả ví dụ: `Android Debug Bridge version 1.0.41`.

### Cài đặt scrcpy
1. **Tải scrcpy**:
   - Tải từ: <https://github.com/Genymobile/scrcpy/releases>
   - Chọn file `.zip` (ví dụ: `scrcpy-win64-v3.3.zip`).
   - Giải nén vào `C:\Program Files\scrcpy-win64-v3.3`.
2. **Thêm vào Path**:
   - Thêm `C:\Program Files\scrcpy-win64-v3.3` vào **Path**.
3. **Kiểm tra**:
   - Chạy:
     ```
     scrcpy --version
     ```
   - Kết quả ví dụ: `scrcpy 3.3`.

### Sử dụng ADB và scrcpy
1. **Kết nối điện thoại**:
   - Kết nối qua USB, bật **Gỡ lỗi USB**.
   - Chạy:
     ```
     adb devices
     ```
   - Nếu thấy serial number với trạng thái `device` (ví dụ: `1234567890abc device`), kết nối thành công.
2. **Chạy scrcpy**:
   - Gõ:
     ```
     scrcpy
     ```
   - Màn hình điện thoại sẽ xuất hiện trên PC.
3. **Tùy chỉnh (tùy chọn)**:
   - Giới hạn độ phân giải: `scrcpy --max-size 1920`.
   - Ghi màn hình: `scrcpy --record file.mp4`.
   - Xem thêm: `scrcpy --help`.

### Khắc phục lỗi
- **ADB không nhận diện thiết bị**:
  - Kiểm tra cáp USB (phải hỗ trợ truyền dữ liệu).
  - Thử cổng USB khác.
  - Cài driver USB từ hãng điện thoại hoặc Universal ADB Driver: <https://adb.clockworkmod.com/>
  - Chạy:
    ```
    adb kill-server
    adb start-server
    adb devices
    ```
- **scrcpy báo lỗi "Could not find any ADB device"**:
  - Đảm bảo `adb devices` liệt kê thiết bị.
  - Cài libusb: <https://libusb.info/>
- **scrcpy lag**:
  - Giảm độ phân giải: `scrcpy --max-size 1280`.
  - Giảm bitrate: `scrcpy --bit-rate 2M`.

---

## 4. Kết Nối Qua Wi-Fi (Tùy Chọn)
1. Kết nối qua USB, chạy:
   ```
   adb tcpip 5555
   ```
2. Ngắt USB, lấy IP điện thoại (vào **Cài đặt > Wi-Fi**).
3. Kết nối:
   ```
   adb connect <IP_điện_thoại>:5555
   ```
4. Chạy `scrcpy`.

---

## 5. Lưu Ý
- **Emulator**: Đảm bảo ảo hóa được bật trong BIOS và driver HAXM/Hypervisor Framework được cài.
- **scrcpy**: Chỉ stream hình ảnh, không stream âm thanh. Chỉ hỗ trợ Android.
- **Cập nhật**: Dùng phiên bản mới nhất của ADB và scrcpy.
- **Lỗi chi tiết**: Chạy `scrcpy -v` để xem log lỗi nếu cần hỗ trợ.

---

## 6. Copyright
© 2025 Phạm Minh Thảo (Trunks-Pham). All rights reserved.  
Version: 2025

---

# English Version

# Guide to Running an Android Emulator and Screen Mirroring a Physical Device

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

## 6. Copyright
© 2025 Phạm Minh Thảo (Trunks-Pham). All rights reserved.  
Version: 2025