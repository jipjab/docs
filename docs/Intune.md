# What is Intune
![Microsoft Intune](img/what-is-intune.png){ width="500" }
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

## Apps Packaging
```title="Template intune Application Infos"
Display Name: Adobe Acrobat Reader DC
Display Version: 23.003.20244.0
Publisher: Adobe Systems Incorporated
Disk Space Required: 500mb

Install: Setup.exe -s /msi /L*v "C:\ProgramData\PG\Logs\Intune-Adobe-Acrobat-Reader-Install.log"
Uninstall: MsiExec.exe /X{AC76BA86-7AD7-FFFF-7B44-AC0F074E4100} /qn /norestart /L*v "C:\ProgramData\PG\Logs\Intune-Adobe-Acrobat-Reader-UnInstall.log"
Detection Method: {AC76BA86-7AD7-FFFF-7B44-AC0F074E4100}
```
```powershell title="Check registry Value"
$KeyPath = "HKCU:\Registry\Key\Path" 
$ValueName = "name" 
$ValueData = "data" 
try{ 
    Get-ItemProperty -Path $KeyPath -Name $valueName -ErrorAction Stop 
} 
catch [System.Management.Automation.ItemNotFoundException] { 
    New-Item -Path $KeyPath -Force New-ItemProperty -Path $KeyPath -Name $ValueName -Value $ValueData -Force 
} 
catch { 
    New-ItemProperty -Path $KeyPath -Name $ValueName -Value $ValueData -Type String -Force 
} 
```

## Proactive Remediations scrips
### Adobe
```powershell title="DETECTION - Adobe Reader Document Bar for Microsoft AIP"
<#
.SYNOPSIS
    PowerShell script to remediate the document message bar in Adobe Reader.
.EXAMPLE
    .\UAT-Win10-01-Remediate-CplIntSettings.ps1
.DESCRIPTION
    This PowerShell script is deployed as a remediation script using Proactive Remediations in Microsoft Endpoint Manager/Intune.
.LINK
https://docs.microsoft.com/en-us/mem/analytics/proactive-remediations
https://helpx.adobe.com/enterprise/kb/mpip-support-acrobat.html
.NOTES
    Version:        1.0
    Creation Date:  July 25, 2023
    Author:         JiPJab
#>

[CmdletBinding()]
Param (
)

$AdobeReaderKeyPath = @("HKCU:\SOFTWARE\Adobe\Acrobat Reader\DC\MicrosoftAIP")
$ValueName = "bShowDMB"  
$ValueData = 1  
try{  
    Get-ItemProperty -Path $AdobeReaderKeyPath -Name $valueName -ErrorAction SilentlyContinue
    Write-host 'Impossible de trouver HKCU:\SOFTWARE\Adobe\Acrobat Reader\DC\MicrosoftAIP.'
    Write-Verbose 'Impossible de trouver HKCU:\SOFTWARE\Adobe\Acrobat Reader\DC\MicrosoftAIP.'
    Exit 1
}
Catch {
    $ErrorMessage = $_.Exception.Message 
    Write-Warning $ErrorMessage
    Exit 1
}
```
```powershell title="REMEDIATION - Adobe Reader Document Bar for Microsoft AIP"
<#
.SYNOPSIS
    PowerShell script to remediate the document message bar in Adobe Reader.
.EXAMPLE
    .\UAT-Win10-01-Remediate-CplIntSettings.ps1
.DESCRIPTION
    This PowerShell script is deployed as a remediation script using Proactive Remediations in Microsoft Endpoint Manager/Intune.
.LINK
https://docs.microsoft.com/en-us/mem/analytics/proactive-remediations
https://helpx.adobe.com/enterprise/kb/mpip-support-acrobat.html
.NOTES
    Version:        1.0
    Creation Date:  July 25, 2023
    Author:         JiPJaB
#>

[CmdletBinding()]
Param (
)

$AdobeReaderKeyPath = @("HKCU:\SOFTWARE\Adobe\Acrobat Reader\DC\MicrosoftAIP")
$ValueName = "bShowDMB"  
$ValueData = 1  
try{
    New-Item -Path $AdobeReaderKeyPath -Force  
    New-ItemProperty -Path $AdobeReaderKeyPath -Name $ValueName -Value $ValueData -Force 
}

catch {  
    $ErrorMessage = $_.Exception.Message 
    Write-Warning $ErrorMessage
    #Exit 1  
}  
```

