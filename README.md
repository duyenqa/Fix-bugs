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
Step 1: [Download HAXM]:(https://github.com/intel/haxm/releases)
Step 2: Extract file in a folder
Step 3: Install android studio to download "Intel x86 Emulator Accelerator (HAXM installer)" in SDK Tools. Error happen!!!
Step 4: Copy HAXM folder and paste in SDK folder
Step 5: Run "haxm-7.8.0-setup.exe" file
Step 6: Back to android studio, continue...
