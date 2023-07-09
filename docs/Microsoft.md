# GPOs
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