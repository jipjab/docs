# Apple

Local account
```sh title="Create a mac os local account"
#!/bin/zsh
# Username and Password to create

username=user
password="pass

# Create User with add to admins

dscl . -create /Users/$username
dscl . -create /Users/$username UserShell /bin/zsh
dscl . -create /Users/$username RealName $username
dscl . -create /Users/$username UniqueID "901"
dscl . -create /Users/$username PrimaryGroupID 20
dscl . -create /Users/$username NFSHomeDirectory /Users/$username
dscl . -passwd /Users/$username $password
dscl . -append /Groups/admin GroupMembership $username

``