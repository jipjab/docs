# Connect on Exchange On-line (Mac)

```ps title="install powershell on a mac"
brew install powershell
brew install openssl
pwsh
Install-Module -Name PowerShellGet
Install-Module -Name PSWSMan
sudo pwsh -Command 'Install-WSMan'
```

```ps title="connect with a mac with an interactive logon"
$ExchangeSession = Connect-ExchangeOnline
```
```ps title="Change calendar time zone"
Set-MailboxCalendarConfiguration -Identity peter@contoso.com -WorkingHoursTimeZone "Pacific Standard Time"
```

## Connect on Microsoft Graph with powershell

```powershell title="Install the module"
Install-Module Microsoft.Graph -Scope AllUsers   
Connect-MgGraph -TenantId "yourTenant GUID"
```
```powershell title="Connect"
Install-Module Microsoft.Graph -Scope AllUsers   
Connect-MgGraph -TenantId "yourTenant GUID"
```
## Test Viruses and Antiviruses
```powershell title="Mimikatz"
invoke-mimikatz
``` 
## Check Installed application
```powershell title="Powershell Check installed applications"
wmic product get name,vendor,IdentifyingNumber,Version,InstallDate
```
## Check running services
```powershell title="powershell check running services"
wmic service get Caption,StartName,StartMode
```
## check installed drivers
```powershell title="Check installed drivers"
driverquery /v
```
## check routes
```powershell title="Check routes"
route print
```
## check tasks
```powershell title="Check computer tasks"
tasklist
```
## Check Boot time
```powershell title="Check Boot time"
systeminfo | find "System Boot Time:"
```

# Autres
```powershell title="How to get date of last Windows update install or at least checked for an update?"
gwmi win32_quickfixengineering |sort installedon -desc 
```
