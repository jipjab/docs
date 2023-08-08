
# Common GPOs
## Allow RDP for Domain admins
## Enable the Remote Desktop Session Host policies
## Enable Network Level Authentication
## Force NumLock on Windows 10
```cmd
Key Path: .DEFAULT\Control Panel\Keyboard
Value data: 2
```
## Creer des filtres GPO pour cibles certains types de machines
Il faut utiliser des filtres WMI

https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/create-wmi-filters-for-the-gpo



