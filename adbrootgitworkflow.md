# 🛠️ Rooted Android Kernel Exploration & Communication Guide

This guide helps you **query the kernel**, **find root-related processes**, and **interact with rooted devices** using **ADB, Termux, and GitHub**. It includes workflows for **multiple rooted devices** to **push, pull, and share kernel modules, patches, and logs** seamlessly.

---

## 🧭 Overview

This guide assumes:
- You have **root access** on your Android device(s).
- You want to **explore kernel processes**, **debug root-related functions**, and **share changes** with other devices or GitHub.
- You may have **multiple rooted devices** that can **communicate** and **sync** with each other and GitHub.

---

## 🔍 Step 1: Querying the Kernel and Root Processes

Use `adb shell` or `termux` on a rooted device to **inspect kernel processes, modules, and root status**.

### 🔍 A. Check Root Status

```bash
su
id
```

- If you see `uid=0(root)`, you have root access.

### 🔍 B. List Kernel Modules

```bash
lsmod
```

- This shows currently loaded kernel modules.

### 🔍 C. View Kernel Logs

```bash
dmesg | grep -i root
```

- Look for kernel messages related to root or security.

### 🔍 D. View Process List

```bash
ps | grep -i root
```

- Shows processes running as root.

### 🔍 E. Check for Root Detection

```bash
cat /proc/self/status | grepUid
```

- If UID is `0`, the process is running as root.

---

## 📡 Step 2: Using ADB with Rooted Devices

### 📡 A. Connect via ADB

```bash
adb connect <device-ip>:5555
```

- Replace `<device-ip>` with the IP of the rooted device.

### 📡 B. Use ADB Shell with Root

```bash
adb shell
su
```

- Now you can run root commands.

### 📡 C. Use ADB to Pull Logs

```bash
adb logcat -d > log.txt
adb pull /data/local/tmp/my_module.ko
```

- Pull logs or kernel modules from the device.

---

## 📱 Step 3: Using Termux on Rooted Devices

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
git commit -m "Add root patch"
```

### 🧰 D. Push to GitHub

```bash
git push origin my-root-patch
```

---

## 🔄 Step 5: Communication Between Rooted Devices

### 🔄 A. Use ADB to Push Files Between Devices

1. From Device A:
   ```bash
   adb push /data/local/tmp/my_module.ko /sdcard/
   ```

2. From Device B:
   ```bash
   adb pull /sdcard/my_module.ko /data/local/tmp/
   ```

### 🔄 B. Use SSH Between Rooted Devices

1. On Device A:
   ```bash
   ssh root@<device-b-ip>
   ```

2. On Device B:
   ```bash
   sshd
   ```

3. From Device A:
   ```bash
   ssh root@<device-b-ip>
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

### 📌 A. Workflow: Kernel Module Testing

1. **Device A** (main dev):
   - Edit kernel module.
   - Commit and push to GitHub.
   - Build and push module to Device B.

2. **Device B** (test device):
   - Pull module from GitHub.
   - Load module using ADB or SSH:
     ```bash
     insmod /data/local/tmp/my_module.ko
     dmesg | tail
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
dmesg | tail
```

### 🧪 C. Use `strace` for Process Tracing

```bash
strace -p <pid>
```

---

## 🔄 Summary

- Use **ADB** to **query kernel processes**, **load modules**, and **pull logs**.
- Use **Termux** for **editing**, **building**, and **testing** kernel modules.
- Use **GitHub** to **share and sync** changes across devices.
- Use **SSH** to **communicate** and **transfer files** between rooted devices.
- Use **multiple rooted devices** to **test, debug, and share** kernel changes efficiently.

This guide helps you **explore, debug, and share kernel work** across multiple rooted devices using **ADB, Termux, and GitHub**. Always ensure you're working with **devices you own or have permission to test**.