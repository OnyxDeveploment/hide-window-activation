# 🖥️ Hide "Activate Windows" Watermark – Complete Guide

This guide explains how to manually or automatically **hide the "Activate Windows" watermark** by modifying registry keys.  
> ⚠️ **This does NOT activate Windows.** It only hides the visual watermark.

---

## 🛠️ Method 1: Manual Registry Editing

### 🔹 Step 1: Open Registry Editor
1. Press **`Windows Key + R`** to open the **Run** dialog.
2. Type **`regedit`** and press **Enter**.
3. If prompted by **User Account Control (UAC)**, click **Yes** to allow changes.

---

### 🔹 Step 2: Navigate to the Activation Key
In the Registry Editor, expand the folders on the left in this order:

HKEY_LOCAL_MACHINE

-> SOFTWARE

-> Microsoft

-> Windows NT

-> CurrentVersion

-> SoftwareProtectionPlatform

-> Activation

> 💡 Tip: If any folder is collapsed (`>` icon), double-click it to expand.

---

### 🔹 Step 3: Modify Registry Values
In the right-hand panel:

1. Locate the entry: **`Manual`**
   - Double-click it.
   - Change **Value data** from `0` to `1`.
   - Click **OK**.

2. Locate the entry: **`NotificationDisabled`**
   - Double-click it.
   - Change **Value data** from `0` to `1`.
   - Click **OK**.

> ⚠️ If these entries don’t exist, this method may not be applicable to your version of Windows.

---

### 🔹 Step 4: Restart Your Computer
To apply changes:
- Close **Registry Editor**
- Restart your PC using one of the following:
  - Start → Power → Restart
  - Or press `Windows + X`, then `U`, then `R`

---

## ⚙️ Method 2: Automated via Batch File

For automation, use a `.bat` script:

### 📄 Step 1: Create the Batch File Or Download it from the page
1. Open **Notepad**
2. Paste the following:

```batch
@echo off
:: Batch file to hide "Activate Windows" watermark by modifying registry keys
:: Run as Administrator for proper execution

:: Check if running as Administrator
net session >nul 2>&1
if %errorLevel% neq 0 (
    echo This script requires Administrator privileges.
    echo Please right-click the file and select "Run as administrator".
    pause
    exit /b
)

:: Set the registry keys to hide the watermark
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\Activation" /v "Manual" /t REG_DWORD /d 1 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\Activation" /v "NotificationDisabled" /t REG_DWORD /d 1 /f

:: Prompt to restart the computer
echo Registry keys have been updated successfully.
echo.
choice /c YN /m "Do you want to restart your computer now to apply changes? (Y/N)"
if errorlevel 2 (
    echo You chose not to restart. Changes will take effect after the next restart.
    pause
    exit /b
)
if errorlevel 1 (
    shutdown /r /t 0
)

```
