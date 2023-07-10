# Tips & Tricks

## GPOs
### View Applied Policies with the Command Prompt
```cmd
gpresult /Scope User /v
gpresult /User xxx /v
gpresult /r
```
// save results to a file
```cmd
Gpresult /R > c:\RsopReport.txt
```
// for a machine (needs to be admin and can be a remote machine)
```cmd
gpresult/r /scope computer
```
[Use GPResult Command to Check Group Policy: Step-by-step Guide (comparitech.com)](https://www.comparitech.com/net-admin/how-to-use-gpresult-command/#:~:text=GPResult%20Scope%20Command,users%2C%20and%20target%20computer%27s%20settings.)


## Windows Licences Activation
### Microsoft Change Product Key Of Office 2016/2013
=> look for a file called **OSPP.VBS**

If you’re running 64-bit Office on 64-bit Windows, use the following command:
```sh
cscript “C:\Program Files\Microsoft Office\Office15\OSPP.VBS”/inpkey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
```
### Activate Windows Server 2019
From a command prompt use 
```sh
cscript c:\windows\system32\slmgr.vbs /ipk <product_key>
```
### Activate Windows Server 2022
From a command prompt use
```sh
Dism /online /Set-Edition:ServerStandard /ProductKey:7QKY9-N6G2J-3W9QY-2PDMF-BY8DW /AcceptEula
```
```sh
cscript c:\windows\system32\slmgr.vbs /ipk GTDN9-YQ2MP-K8QYD-GPXMT-XQDVR
```
## Run Computer management as admin
```sh
compmgmt.msc
```
## Windows System UI Languages
### To See System Default UI Language of Windows 10 in PowerShell
```sh
Get-Culture | Format-List -Property *
```
### To See System Default UI Language of Windows 10 in Command Prompt (as admin)
```sh
dism /online /get-intl
```
### To See System Default and Installed Language of Windows 10 in Registry Editor
In Registry Editor, browse to the key location below. (see screenshot below)
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Language
## How to See Currently Logged in Users in Windows 10 / 8 / 7
```sh
query user
```
## See Windows Installed versions
```sh
systeminfo
```
## Connaitre l'IP public de sortie d'une machine par command
nslookup myip.opendns.com resolver1.opendns.com
## Command to reboot windows computer
```sh
shutdown /r
```
=> The above command will set a time out of 30 seconds to close the applications. After 30 seconds, windows reboot will start.
## Change user password in Windows 10!
cmd.exe in administrative mode [Windows-Logo+X].
```sh
net user
```
See all Windows-10 User Accounts.

Then type in the command (in this case for the administrator account): 
```sh
net user administrator *
```
And now enter the new Windows 10 password and retype the password to confirm the password.
This creates the user account:
```sh
net user /add [username] [password]
```
This adds the user to the Local Administrators Group
```sh
net localgroup administrators [username] /add
```
## How to force Windows 10 time to synch with a time server?
### Method 1:
Press Windows key + r and type **services.msc** and press enter.
Right click on Windows Time and select properties to check the status of the service.
Restart the Windows Time service.
Click on OK.
Restart the computer
### Method 2:
Click on clock and select “Change date and time settings”.
Click on the “Internet Time” tab.
Check if it is set to “synchronize the time with time.windows.com”
If the option is selected, click on change settings to check the option “Synchronize with an Internet Time server”
Click on OK.
### Method 3:
Press Windows key + X and select Command prompt(Admin).
Type each one of the command below and press enter.
```ps
net stop w32time
```
```ps
w32tm /unregister
```
```ps
w32tm /register
```
```ps
net start w32time
```
```ps
w32tm /resync
```
Restart the computer to test the issue again.
W32tm.exe is used to configure Windows Time service settings. It can also be used to diagnose problems with the time service. W32tm.exe is the preferred command line tool for configuring, monitoring, or troubleshooting the Windows Time service.