# Fix Bluetooth After Sleep (Windows 11)

This guide explains how to automatically run a batch script (**fix_bluetooth.bat**) every time your computer wakes up from sleep. The script restarts the Bluetooth stack, services, and device drivers to restore Bluetooth functionality without requiring a reboot.

---

## 🛠️ **Steps to Run a Script When the Computer Wakes Up**

### 1️⃣ **Open Task Scheduler**
1. **Press `Win + R`**, type **taskschd.msc**, and press **Enter**.  
2. In the Task Scheduler window, click **Create Task** on the right side.

---

### 2️⃣ **General Settings**
1. **Name**: Enter a name like **Fix Bluetooth After Sleep**.  
2. **Description** (optional): Write something like **"Runs fix_bluetooth.bat after waking up from sleep."**  
3. **Check** the box **Run with highest privileges** (this allows the script to control services and devices).  
4. **Select "Configure for:"** to **Windows 11** (or **Windows 10** if that’s what you see).  

---

### 3️⃣ **Triggers (When to Run)**
1. Go to the **Triggers** tab and click **New...**.  
2. **Begin the task**: Select **On an event**.  
3. **Settings**:  
   - **Log**: Choose **System**.  
   - **Source**: Choose **Power-Troubleshooter**.  
   - **Event ID**: Enter **1** (this event ID corresponds to "wake from sleep").  
4. Click **OK**.  

---

### 4️⃣ **Actions (What to Run)**
1. Go to the **Actions** tab and click **New...**.  
2. **Action**: Select **Start a program**.  
3. **Program/Script**: Browse to the location of your **fix_bluetooth.bat** file.  
4. Click **OK**.  

---

### 5️⃣ **Conditions (When to Allow It)**
1. Go to the **Conditions** tab.  
2. **Uncheck** "Start the task only if the computer is on AC power" (so it runs even on battery).  
3. You can leave the rest as-is.  

---

### 6️⃣ **Settings (How to Run It)**
1. Go to the **Settings** tab.  
2. **Check** "Allow task to be run on demand."  
3. **Check** "If the task fails, restart every" and set it to **1 minute, 3 retries** (just in case).  
4. Click **OK** to finish and save the task.  

---

## 📜 **Summary of What Happens**
- When your computer wakes up from sleep, **Event ID 1** from **Power-Troubleshooter** triggers the task.  
- Task Scheduler runs **fix_bluetooth.bat** with administrative privileges.  
- The Bluetooth stack, drivers, and services are automatically reset.  

This setup ensures you **never have to run the script manually** again.

---

## 📄 **fix_bluetooth.bat**

Here's the batch script used to reset the Bluetooth stack, services, and drivers.

```batch
@echo off

:: Restart Bluetooth Support Service
net stop "Bluetooth Support Service"
timeout /t 3
net start "Bluetooth Support Service"

:: Disable Bluetooth Adapter
powershell -Command "Get-PnpDevice | Where-Object { $_.FriendlyName -like '*Bluetooth*' } | Disable-PnpDevice -Confirm:$false"
timeout /t 3

:: Enable Bluetooth Adapter
powershell -Command "Get-PnpDevice | Where-Object { $_.FriendlyName -like '*Bluetooth*' } | Enable-PnpDevice -Confirm:$false"

:: End
echo Done.
pause
```

---

With this configuration, your computer will automatically restore Bluetooth functionality every time it wakes up from sleep. Let me know if you'd like any adjustments or explanations. 😊

