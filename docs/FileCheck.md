## File checks in powershell

```powershell title="A basic file check (in case you’re feeling scripty)"
$File = "C:\windows\system32\notepad.exe"
if (Test-Path $File) {
    write-output "Notepad detected, exiting"
    exit 0
}
else {
    exit 1
}
```

```powershell title="A registry key (checking both it exists and the value is correct, good for versioning)"
$Path = "HKLM:\SOFTWARE\7-Zip"
$Name = "Path"
$Type = "STRING"
$Value = "C:\Program Files\7-Zip\"

Try {
    $Registry = Get-ItemProperty -Path $Path -Name $Name -ErrorAction Stop | Select-Object -ExpandProperty $Name
    If ($Registry -eq $Value){
        Write-Output "Detected"
       Exit 0
    } 
    Exit 1
} 
Catch {
    Exit 1
}
```

```powershell title="Check if a service exists"
$service = get-service -name "MozillaMaintenance"
if ($service) {
    write-output "MozillaMaintenance detected, exiting"
    exit 0
}
else {
    exit 1
}
```

```powershell title="Service exists and is running"
$service = get-service -name "MozillaMaintenance"
if ($service.Status -eq "Running") {
    write-output "MozillaMaintenance detected and running, exiting"
    exit 0
}
else {
   exit 1
}
```

```powershell title="Both file and service exist"
$File = "C:\windows\system32\notepad.exe"
$service = get-service -name "MozillaMaintenance"
if (($service) -and (Test-Path $File)) {
    write-output "MozillaMaintenance and Notepad detected, exiting"
    exit 0
}
else {
    exit 1
}
```

```powershell title="File is of a certain version"
$fileversion = (Get-Command C:\Windows\System32\notepad.exe).Version
$build = 19041
$detectedbuild = $fileversion.Build
if ($detectedbuild -eq $build) {
    write-output "Detected"
    exit 0
}
else {
    exit 1
}
```

```powershell title="Or even the file was last modified today"
$filedate = (Get-Item C:\Windows\System32\notepad.exe).LastWriteTime
if ($filedate -gt (Get-Date).AddDays(-1)) {
    write-output "Detected"
    exit 0
}
else {
    exit 1
}
```

```powershell title="When looking at logged in user, we need to find their details so for this I have a new function"
function getloggedindetails() {
    ##Find logged in username
    $user = Get-WmiObject Win32_Process -Filter "Name='explorer.exe'" |
      ForEach-Object { $_.GetOwner() } |
      Select-Object -Unique -Expand User
    
    ##Find logged in user's SID
    ##Loop through registry profilelist until ProfileImagePath matches and return the path
        $path= "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\*"
        $sid = (Get-ItemProperty -Path $path | Where-Object { $_.ProfileImagePath -like "*$user" 
}).PSChildName

    $return = $sid, $user
    
    return $return
    }
```

```powershell title="Running this will give you the username and SID. For our 7-zip registry example:"
$loggedinuser = getloggedindetails
##Set key

$sid = $loggedinuser[0]
$Path = "Registry::HKU\$sid\SOFTWARE\7-Zip"
$Name = "Path"
$Type = "STRING"
$Value = "C:\Program Files\7-Zip\"

Try {
    $Registry = Get-ItemProperty -Path $Path -Name $Name -ErrorAction Stop | Select-Object -ExpandProperty $Name
    If ($Registry -eq $Value){
        Write-Output "Detected"
       Exit 0
    } 
    Exit 1
} 
Catch {
    Exit 1
        write-host "not detected"
    write-host $path
}
```

```powershell title="Or for a file at the user level:"
$username = $loggedinuser[1]
$File = "C:\users\$username\AppData\Local\Microsoft\Teams\update.exe"
if (Test-Path $File) {
    write-output "Teams Update detected, exiting"
    exit 0
}
else {
    exit 1
}
```
