# Fix-bugs

## Bug 1
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
