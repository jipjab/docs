## Run a health check script
Run the Exchange HealthChecker script (https://microsoft.github.io/CSS-Exchange/Diagnostics/HealthChecker/) , and check the build number.

## Find Exchange version with PowerShell
```powershell title="We are going to use the Get-ExchangeServer cmdlet." 
Get-ExchangeServer | Format-List Name, Edition, AdminDisplayVersion
```
```powershell title="Find Exchange version with PowerShell including Security Update" 
$ExchangeServers = Get-ExchangeServer | Sort-Object Name
ForEach ($Server in $ExchangeServers) {
    Invoke-Command -ComputerName $Server.Name -ScriptBlock { Get-Command Exsetup.exe | ForEach-Object { $_.FileversionInfo } }
}
```
## Find Exchange Product name from Build number
Go to the Microsoft Docs (https://learn.microsoft.com/en-us/exchange/new-features/build-numbers-and-release-dates?view=exchserver-2019) page and check the Product name

