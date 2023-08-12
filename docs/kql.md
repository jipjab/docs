# Advanced hunting commands - KQL tips

```kql title="Shows known domain controllers"
//Show Domain Controllers using localport 88 - type FourToSixMapping
DeviceNetworkEvents
| where LocalPort == "88"
| where LocalIPType contains "FourToSixMapping"
| distinct DeviceId
| join 
    (DeviceInfo
    | summarize arg_max(Timestamp,*) by DeviceId
    )
on DeviceId
| project DeviceId, DeviceName, OnboardingStatus
```

Except if your DCs are used as files server, which is of course not recommended at all you should not see many files copied from a workstation or member server to DCs.

Using this KQL query you can monitor this activity and identify potential suspect activities or even risky activities:

```kql title="List of files copied from a client to DCs over the last 30 days"
IdentityDirectoryEvents
| where ActionType == @"SMB file copy"
| extend ParsedFields=parse_json(AdditionalFields)
| project Timestamp, ActionType, DeviceName, IPAddress, AccountDisplayName, DestinationDeviceName, DestinationPort, FileName=tostring(ParsedFields.FileName), FilePath=tostring(ParsedFields.FilePath), Method=tostring(ParsedFields.Method)
| where Method == @"Write"
```
Even with a password policy in place that affects all user accounts, it is possible to set a blank password for a user with the setting “Account Password Not Required” using for example a PowerShell cmdlet (not possible through the GUI). This is why it's important to list all users with this setting enabled using the MDI portal but also to track all changes from “Account Password Not Required” = FALSE to TRUE:

```kql title="“Account Password Not Required” changed from FALSE to TRUE"
IdentityDirectoryEvents
| where ActionType == @"Account Password Not Required changed"
| extend PreviousValue = todynamic(AdditionalFields)["FROM Account Password Not Required"]
| extend NewValue = todynamic(AdditionalFields)["TO Account Password Not Required"]
| where "True"==NewValue
| project Timestamp, ActionType, Application, TargetAccountDisplayName, PreviousValue, NewValue
```

```kql title="Detect when a local user account is created on an endpoint"

//Data connector required for this query - M365 Defender - Device* tables

//Microsoft Sentinel query
DeviceEvents
| where TimeGenerated > ago(7d)
| where ActionType == "UserAccountCreated"
//Exclude defaultuser1 which is created by Windows through different processes 
| where AccountName != "defaultuser1"
| project
    TimeGenerated,
    DeviceName,
    ['Account Created Name']=AccountName,
    Actor=InitiatingProcessAccountName

//Advanced Hunting query

//Data connector required for this query - Advanced Hunting license

DeviceEvents
| where Timestamp > ago(7d)
| where ActionType == "UserAccountCreated"
//Exclude defaultuser1 which is created by Windows through different processes 
| where AccountName != "defaultuser1"
| project
    Timestamp,
    DeviceName,
    ['Account Created Name']=AccountName,
    Actor=InitiatingProcessAccountName
```

The account lockout policy is a built-in security measure that limits malicious users and hackers from illegitimately accessing your network resources. However, employees often use multiple devices, numerous productivity applications, Windows services, tasks, network mapping and more, which can store a wrong password and set off the account lockout.
It could be interesting to identify machines or IPs from where Account Lockout threshold is triggered only based on MDI raw data.
Remark: DeviceName and IPAdress can sometime be empty (no raw data).

```kql title="Tips 12 – Identify machines or IPs from where Account Lockout threshold is triggered"
IdentityLogonEvents
| where Application == @"Active Directory" // AD only
| where AccountDomain == @"msdemo.org" // if needed to filter by domain
| where ActionType == @"LogonFailed"
| where FailureReason == @"WrongPassword" or FailureReason == @"AccountLocked" //badpasswordcount attribute
| summarize FailureReason = count() by DeviceName, IPAddress, AccountUpn
| where FailureReason > 15 //depending on the Account Lockout threshold
```

See: https://learn.microsoft.com/en-us/microsoft-365/security/defender/custom-detection-rules?view=o365-worldwide
