# Window GPOs
## View Applied Policies with the Command Prompt
```powershell
gpresult /Scope User /v
gpresult /User xxx /v
gpresult /r

// save results to a file
Gpresult /R > c:\RsopReport.txt

// for a remote machine
gpresult/s COMPUTERNAME

// for a machine (needs to be admin)
gpresult/r /scope computer
```
#### Ressources:
[Use GPResult Command to Check Group Policy: Step-by-step Guide (comparitech.com)](https://www.comparitech.com/net-admin/how-to-use-gpresult-command/#:~:text=GPResult%20Scope%20Command,users%2C%20and%20target%20computer%27s%20settings.)

# Common GPOs
## Allow RDP for Domain admins
## Enable the Remote Desktop Session Host policies
## Enable Network Level Authentication
## Force NumLock on Windows 10
```sh
Key Path: .DEFAULT\Control Panel\Keyboard
Value data: 2
```
## WinRM with domain controller Group Policy
