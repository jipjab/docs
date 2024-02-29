# To install sudo on Debian, follow these steps:
## Install the 'sudo' Command:
Log in as root and update the system package list using apt update.
Install sudo by running apt install sudo or combine both commands with 

```
apt update && apt install sudo
```

## Add a Regular User to the sudo Group:
Add a user to the sudo group by editing "sudoers"
```
nano /etc/sudoers
```
### User privilege specification
root    ALL=(ALL:ALL) ALL
jip     ALL=(ALL:ALL) ALL
