# Fix-bugs

## Bug 1 - Error Appium Inspector when start session
> Failed to create session. An unknown server-side error occurred while processing the command. Original error: Error executing adbExec. Original error: 'Command 'C:\\Users\\Admin\\Documents\\Learning24\\SDK\\platform-tools\\adb.exe -P 5037 -s emulator-5554 forward tcp\:8200 tcp\:6790' exited with code 1'; Command output: adb.exe: error: cannot bind listener: cannot bind to 127.0.0.1:8200: **An attempt was made to access a socket in a way forbidden by its access permissions.(10013)**

**Solution:** Restarting the Host Network Service on Windows

- Run cmd 1 as administrator:

```txt
net stop hns
net start hns
```

- Run cmd 2:

```txt
appium --alow-cors
```

## Bug 2 - Install android studio 2024
> Intel HAXM installation failed!
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

**Refer detail:** [Youtube](https://www.youtube.com/watch?v=EwbNr_rmcwI)

## Bug 3 - Start Appium in Command Prompt
> Could not start REST http interface listener
Could not configure Appium server. It's possible that a driver or plugin tried to update the server and failed. Original error: listen EACCES: permission denied 0.0.0.0:4723

**Solution:**

Restart the computer 

## Bug 4 - Read Data from excel file to web
> Error: java.lang.IllegalArgumentException: Keys to send should be a not null CharSequence
> 
> TestLogin[null,null]

| TC  | Username  | Password |
|-----|-----------|----------|
|TC01 |  test     |  123456  |
|TC01 |  test123  |  123456  |
|TC01 |  admin    |  demo123 |

```java
@DataProvider(name = "LoginDataProvider")
public Object[][] LoginData() {
    String[][] table = null;
    try {
        FileInputStream fileInputStream = new FileInputStream(new File("D:\\login.xlsx"));
        XSSFWorkbook xssfWorkbook = new XSSFWorkbook(fileInputStream);
        XSSFSheet xssfSheet = xssfWorkbook.getSheet("Sheet1");
    
        int startRow = 1;
        int totalRows = xssfSheet.getLastRowNum();
        table = new String[totalRows][2];
    
        for (int i = startRow; i <= totalRows; i++) {
            for (int j = 1; j <= 2; j++) {
                XSSFCell cell = xssfSheet.getRow(i).getCell(j);
                if (cell != null) {
                    String cellValue = cell.getStringCellValue();
                    table[i][j] = cellValue;
                    System.out.println(cellValue);
                } else {
                    System.out.println("Error data!!! Cell at row " + i + ", column " + j + " is null.");
                }
            }
        }
    } catch (Exception e) {
        System.out.println(e.getMessage());
    }
    return table;
}
```

**Fix way 1**
```java
...
table[i-1][j-1] = cellValue;
...
```

**Fix fix way 2**
```java
@DataProvider(name = "LoginDataProvider")
public Object[][] LoginData() {
    String[][] table = null;
    
    try {
        FileInputStream fileInputStream = new FileInputStream(new File("D:\\login.xlsx"));
        XSSFWorkbook xssfWorkbook = new XSSFWorkbook(fileInputStream);
        XSSFSheet xssfSheet = xssfWorkbook.getSheet("Sheet1");
    
        int startRow = 1;
        int ci = 0, cj = 0;
        int totalRows = xssfSheet.getLastRowNum();
        table = new String[totalRows][2];
    
        for (int i = startRow; i <= totalRows; i++, ci++) {
            for (int j = 1; j <= 2; j++, cj++) {
                XSSFCell cell = xssfSheet.getRow(i).getCell(j);
                if (cell != null) {
                    String cellValue = cell.getStringCellValue();
                    table[ci][cj] = cell.getStringCellValue();
                    System.out.println(cellValue);
                } else {
                    System.out.println("Error data!!! Cell at row " + i + ", column " + j + " is null.");
                }
            }
        }
    } catch (Exception e) {
        System.out.println(e.getMessage());
    }
    return table;
}
```

**Fix way 3**
- Adjust excel file
- _**This is not recommended**_, if you edit what was originally, the interviewer will let you fail
  
| Username  | Password |
|-----------|----------|
|  test     |  123456  |
|  test123  |  123456  |
|  admin    |  demo123 |

```java
@DataProvider(name = "LoginDataProvider")
public Object[][] LoginData() {
    String[][] table = null;
    
    try {
        FileInputStream fileInputStream = new FileInputStream(new File("D:\\login.xlsx"));
        XSSFWorkbook xssfWorkbook = new XSSFWorkbook(fileInputStream);
        XSSFSheet xssfSheet = xssfWorkbook.getSheet("Sheet1");
    
        int startRow = 1;
        int totalRows = xssfSheet.getLastRowNum();
        table = new String[totalRows][2];
    
        for (int i = startRow; i <= totalRows; i++) {
            for (int j = 0; j < 2; j++) {
                XSSFCell cell = xssfSheet.getRow(i).getCell(j);
                if (cell != null) {
                    String cellValue = cell.getStringCellValue();
                    table[i - 1][j] = cellValue;
                    System.out.println(cellValue);
                } else {
                    System.out.println("Error data!!! Cell at row " + i + ", column " + j + " is null.");
                }
            }
        }
    } catch (Exception e) {
        System.out.println(e.getMessage());
    }
    return table;
}
```
**Fix way 4**
- Don't read file excel
  
```java
@DataProvider(name = "LoginDataProvider")
public Object[][] LoginData() {
    return new Object[][]{
            {"test", "123456"},
            {"test123", "123456"},
            {"admin", "demo123"}
    };
}
```

## Bug 5 - Remove database
> Cannot drop database "Salesdb" because it is currently in use.

**Solution:**
```sql
USE master
GO

ALTER DATABASE Salesdb SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
GO
DROP DATABASE Salesdb;
```

## Author
By Ngô Thị Kim Duyên - 2024
