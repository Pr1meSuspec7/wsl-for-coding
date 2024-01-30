# Configuring WSL for coding

## Requirements

- Windows 11 machine
- The **Virtualization Technology (VT-x)** must be enabled in bios configuration.

## Install WSL

### 1. Open PowerShell as administrator and run:

```
wsl --install
```
```
...
Installing: Virtual Machine Platform
Virtual Machine Platform has been installed.
Installing: Windows Subsystem for Linux
Windows Subsystem for Linux has been installed.
Installing: Ubuntu
Ubuntu has been installed.
The requested operation is successful. 
Changes will not be effective until the system is rebooted.
```

### 2. Reboot

### 3. Check status
```
wsl --status
```
```
...
Versione predefinita: 2
```

:warning:WARNING: If you get an error like this:
```
WslRegisterDistribution failed with error: 0x8004032d
Error: 0x8004032d (null)
```

Run this command, reboot then check again (step 3):
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### 4. Install ubuntu

Open a terminal, run "ubuntu" command and follow instruction to create user:

```
...
PS C:\WINDOWS\system32> ubuntu
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers

Enter new UNIX username:      <-- insert username
New password:                 <-- insert password
Retype new password:          <-- repeat password

passwd: password updated successfully
Installation successful!
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.133.1-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


This message is shown once a day. To disable it please create the
/home/mark/.hushlogin file.
mark@NTT-G09LQQ2:~$
```

### 5. Update ubuntu

Run:
```
sudo apt update ; sudo apt upgrade -y
```

### 6. Install useful tools

```
sudo apt install -y nmap nano vim tcpdump net-tools ipcalc neofetch netcat traceroute
```

### 7. Install VScode

https://code.visualstudio.com/

### 8. Install WSL extension

Open VScode and after few seconds you should see this notification in the bottom right corner:
```
You have Windows Subsystem for Linux (WSL) installed on your system. 
Do you want to install the recommended 'WSL' extension from Microsoft for it?
```
You can click "install" or move on extension and search for "WLS"

### 9. Use ubuntu in VScode

Click the "Remote Window Icon" in the bottom left corner and select "Connect to WSL".  
Now you are in the ubuntu machine.


## Network setup

### 1. Change subnet for WSL machines

WSL's default subnet is too large and may overlap with some client networks.  
To reduce this network, two registry keys must be changed.

Press Win-key, digit "regedit" and press enter.  
Go to this path and change these keys:
```
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss\
> NatNetwork
> NatGatewayIpAddress
```

I recommend using:
- NatNetwork: 192.168.255.0/24
- NatGatewayIpAddress: 192.168.255.1

Then reboot.

### 2. Run ubuntu again

The next time you run ubuntu you will see an error, don't worry, this is because you have changed the default subnet.  
This error will no longer appear on future runs.

### 3. Check new ip address

Run ubuntu and digit "ifconfig" or "ip address" command, you should see the new ip address in the new subnet.

## Appendix

[How to install WSL](https://learn.microsoft.com/en-us/windows/wsl/install)  
[Best practices for setting up a WSL](https://learn.microsoft.com/en-us/windows/wsl/setup/environment)  
