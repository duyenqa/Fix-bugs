# Fix-bugs

## Bug 1 - Error Appium Inspector when start session
Failed to create session. An unknown server-side error occurred while processing the command. Original error: Error executing adbExec. Original error: 'Command 'C:\\Users\\Admin\\Documents\\Learning24\\SDK\\platform-tools\\adb.exe -P 5037 -s emulator-5554 forward tcp\:8200 tcp\:6790' exited with code 1'; Command output: adb.exe: error: cannot bind listener: cannot bind to 127.0.0.1:8200: **An attempt was made to access a socket in a way forbidden by its access permissions.(10013)**

**Solution:** Restarting the Host Network Service on Windows

- Open cmd 1:

```txt
net stop hns
net start hns
```

- Open cmd 2:

```txt
appium --alow-cors
```

## Bug 2 - Install android studio 2024
Intel HAXM installation failed!
For more details, please check the installation log: C:\Users\Admin\Appdata\Local\Temp\haxm_install-20240928_1614.log
Intel HAXM installation failed. To install Intell HAXM follow the instructions found at: https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows

**Solution:**
Check Hyper-V in windows. By enter "Turn Windows features on or off" in Start. 

==> Missing Hyper-V in windows 11 Home

**Step 1:** Create a txt file. Example: "hyperv.txt"

```txt
pushd "%~dp0"

dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt

for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"

del hyper-v.txt

Dism /online /enable-feature /featurename:Microsoft-Hyper-V -All /LimitAccess /ALL

pause
```
**Step 2:** Save As "hyperv.txt" 
**Step 3:** Save As "hyper.bat" --> Run as administrator

**Step 4:**
Back to "Turn Windows features on or off":
- Unchecked: Hyper-V
- Checked: Virtual Machine Platform
- Checked: Windows Hypervisor Platform

**Step 5:** [Download HAXM]:(https://github.com/intel/haxm/releases)

**Step 6:** Extract file in a folder

**Step 7:** Install android studio to download "Intel x86 Emulator Accelerator (HAXM installer)" in SDK Tools. Error happen!!!

**Step 8:** Copy HAXM folder and paste in SDK folder

**Step 9:** Run "haxm-7.8.0-setup.exe" file

**Step 10:** Back to android studio, continue...



