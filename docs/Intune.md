# What is Intune
![Microsoft Intune](img/what-is-intune.png)
## Manually register devices for Windows Autopilot
ALT + F10 on windows boot apres avoir selectionner la bonne langue de clavier

```cmd title="Start powershell from CMD"
PowerShell.exe
```
```powershell title="Install Get-WindowsAutopilotInfo.ps1"
Install-Script -name Get-WindowsAutopilotInfo -Force
```
```powershell title="Change the persimission to allow the scripts to run"
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
```
```powershell title="Register the device"
Get-WindowsAutopilotInfo -Online
```
> References:</br>
[Manually register devices with Windows Autopilot | Microsoft Learn](https://learn.microsoft.com/en-us/mem/autopilot/add-devices)</br>