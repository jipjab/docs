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

## Check if an email address is in the Blocked Senders

To check if an email address is in the **Blocked Senders list** in **Outlook** for a user using **Exchange Online (Microsoft 365)**, you have a few options depending on your access level:

If you're an Exchange Online administrator, you can use **PowerShell** to check a user's junk email configuration, including the blocked senders list.

#### **Steps:**

1. **Connect to Exchange Online PowerShell:**

```powershell
Connect-ExchangeOnline -UserPrincipalName youradmin@domain.com
```

2. **Get the user's junk email configuration:**

```powershell
Get-MailboxJunkEmailConfiguration -Identity user@domain.com
```

3. **Check the `BlockedSendersAndDomains` property:**

This command shows the list of blocked email addresses/domains:

```powershell
(Get-MailboxJunkEmailConfiguration -Identity user@domain.com).BlockedSendersAndDomains
```

4. **Search for a specific email address or domain:**

```powershell
(Get-MailboxJunkEmailConfiguration -Identity user@domain.com).BlockedSendersAndDomains -contains "example@bad.com"
```

> This will return `True` or `False`.

### 🔒 Notes

* The `BlockedSendersAndDomains` list is **user-specific**, not global.
* Users can block up to **1024 entries** in their list.
* Admins cannot directly change this list without using PowerShell (no UI control in Microsoft 365 admin center).


