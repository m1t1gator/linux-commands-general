# 🛠️ Non-Root Android Kernel Exploration with Shizuku & GitHub

This guide helps you **explore kernel processes**, **debug root-related functions**, and **interact with non-root devices** using **Shizuku, ADB, Termux, and GitHub**. It includes workflows for **multiple devices** to **push, pull, and share kernel modules, patches, and logs** seamlessly — **without root access**.

---

## 🧭 Overview

This guide assumes:
- You have **no root access** on your Android device(s).
- You want to **explore kernel processes**, **debug root detection**, and **share kernel patches**.
- You may have **multiple devices** that can **communicate** and **sync** with each other and GitHub using **Shizuku** for temporary elevated access.

---

## 🔍 Step 1: Querying the Kernel and Root Detection

Use **ADB shell** or **Termux** on a non-rooted device to **inspect kernel processes, modules, and root detection**.

### 🔍 A. Check for Root Detection

```bash
adb shell
cat /proc/self/status | grepUid
```

- If UID is `1000`, the process is running as a regular user.

### 🔍 B. List Kernel Modules

```bash
adb shell lsmod
```

- This shows currently loaded kernel modules.

### 🔍 C. View Kernel Logs

```bash
adb logcat -d > log.txt
```

- Look for kernel messages related to security or root detection.

### 🔍 D. View Process List

```bash
adb shell ps | grep -i root
```

- Shows any processes that might be trying to run as root.

---

## 📡 Step 2: Using ADB with Non-Root Devices

### 📡 A. Connect via ADB

```bash
adb connect <device-ip>:5555
```

- Replace `<device-ip>` with the IP of the non-rooted device.

### 📡 B. Use ADB Shell

```bash
adb shell
```

- Run standard commands like `ls`, `cat`, `dmesg`, and `logcat`.

### 📡 C. Use ADB to Pull Logs

```bash
adb logcat -d > log.txt
adb pull /data/data/com.termux/files/home/kernel_module.ko
```

- Pull logs or kernel modules from the device.

---

## 📱 Step 3: Using Termux on Non-Root Devices

### 📱 A. Install Termux

- From Play Store or F-Droid.

### 📱 B. Install Required Packages

```bash
pkg update && pkg upgrade
pkg install git openssh python clang make perl ncurses-utils wget unzip rsync vim
```

### 📱 C. Set Up SSH Keys

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

- Add the public key to GitHub or another device.

---

## 🧰 Step 4: GitHub Integration for Kernel Work

### 🧰 A. Clone Kernel or Module Repo

```bash
git clone https://github.com/<username>/<kernel-module>.git
cd <kernel-module>
```

### 🧰 B. Create a New Branch

```bash
git checkout -b my-root-patch
```

### 🧰 C. Make Changes and Commit

```bash
# Edit files
git add .
git commit -m "Add root detection patch"
```

### 🧰 D. Push to GitHub

```bash
git push origin my-root-patch
```

---

## 🔄 Step 5: Communication Between Non-Root Devices

### 🔄 A. Use ADB to Push Files Between Devices

1. From Device A:
   ```bash
   adb push /data/data/com.termux/files/home/kernel_module.ko /sdcard/
   ```

2. From Device B:
   ```bash
   adb pull /sdcard/kernel_module.ko /data/data/com.termux/files/home/
   ```

### 🔄 B. Use SSH Between Devices (If Rooted)

- If you have a **rooted device**, use SSH to transfer files:
  ```bash
  ssh root@<device-ip>
  ```

---

## 📁 Step 6: Syncing and Sharing Between Devices

### 📁 A. Use GitHub to Share Patches

- Push changes to GitHub and pull on other devices:
  ```bash
  git pull origin my-root-patch
  ```

### 📁 B. Use Termux on All Devices

- Sync Termux repos across devices using `rsync` or `scp`:
  ```bash
  rsync -avz /data/data/com.termux/files/home/ user@<device-ip>:/data/data/com.termux/files/home/
  ```

---

## 📌 Step 7: Practical Workflow Example

### 📌 A. Workflow: Kernel Module Testing (Non-Root)

1. **Device A** (main dev):
   - Edit kernel module.
   - Commit and push to GitHub.
   - Build and push module to Device B.

2. **Device B** (test device):
   - Pull module from GitHub.
   - Load module using ADB or Termux:
     ```bash
     adb push kernel_module.ko /data/local/tmp/
     adb shell insmod /data/local/tmp/kernel_module.ko
     ```

3. **Device A**:
   - Pull logs from Device B:
     ```bash
     adb pull /data/local/tmp/log.txt
     ```

---

## 🧪 Step 8: Debugging and Testing

### 🧪 A. Use `logcat` for App Logs

```bash
adb logcat -d > app_log.txt
```

### 🧪 B. Use `dmesg` for Kernel Logs

```bash
adb shell dmesg | tail
```

### 🧪 C. Use `strace` for Process Tracing

```bash
adb shell strace -p <pid>
```

---

## 🔄 Summary

- Use **ADB** to **query kernel processes**, **load modules**, and **pull logs**.
- Use **Termux** for **editing**, **building**, and **testing** kernel modules.
- Use **GitHub** to **share and sync** changes across devices.
- Use **Shizuku** (on rooted devices) or **ADB** (on non-rooted) to **communicate** and **transfer files** between devices.
- Use **multiple devices** to **test, debug, and share** kernel changes efficiently.

This guide helps you **explore, debug, and share kernel work** across multiple **non-rooted and optionally rooted devices** using **ADB, Termux, GitHub, and Shizuku**. Always ensure you're working with **devices you own or have permission to test**.