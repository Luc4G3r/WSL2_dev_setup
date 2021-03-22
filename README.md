# WSL2_dev_setup
Setup instructions WSL2 for web development (XServer and Windows GUI variants)
* Goals:
  * Running apache web server on WSL2
  * Running PHPStorm on WSL2 with display on Windows 10 XServer
* Reasons:
  * Virtual Machine Performance issues
  * Dual desktops and switching between host machine and virtual machine disturbs workflows

NOTE: This instruction manual is still work in progress.
## WSL
[Microsoft's instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10)  
[Microsoft's instructions (GER)](https://docs.microsoft.com/de-de/windows/wsl/install-win10)
* [Check WSL2 System requirements](https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-2---check-requirements-for-running-wsl-2)
* In Windows Powershell, run:
```
 dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
This activates WSL Version 1 on your system
### Hyper V
* In Windows Powershell, run:
```
 dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
This enables HyperV.
NOTE: HyperV is not compatible with VMWare Workstation
* To disable hypervisor (and run VMWare), run:  
 `bcdedit /set hypervisorlaunchtype off`
* To enable hypervisor, run:  
 `Enable Windows Feature Hyper-V` // only required once  
 `bcdedit /set hypervisorlaunchtype auto`
Restart will be needed for changes to take place.
* In Windows User directory, create or update `.wslconfig`:  
```
 [wsl2]
 memory=10GB
 processors=8
 swap=10GB
 localhostForwarding=true
```
  * Change these values according to your needs / specs (online localhostForwarding option is mandatory)
### WSL2
* [Download Linux kernel update package](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)  
_[Click here if link above is not working](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)_
* Run the `.msi` file as administrator
* In Windows Powershell, run:
```
 wsl --set-default-version 2
```

### Ubuntu 20 LTS installation
* To install Ubuntu 20 LTS or any other of the [supported distributions](https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-6---install-your-linux-distribution-of-choice) open [Windows Store](https://aka.ms/wslstore)
* Launch your distribution!

## XServer
### Install VcXsrv (XServer, inside Windows)
[Show VcXsrv Setup instructions](https://github.com/Luc4G3r/WSL2_dev_setup/blob/main/XServer/VCXSRV_SETUP.md)
### Install terminator (Example to run on XServer, inside Ubuntu) - optional
[Show Terminator Setup instructions](https://github.com/Luc4G3r/WSL2_dev_setup/blob/main/XServer/TERMINATOR_SETUP.md)
### PHPStorm install WSL2
* Windows PHPStorm indexing of wsl path projects is very slow, [Jetbrains is working on this](https://youtrack.jetbrains.com/issue/IDEA-240351)  
[Follow systemd installation instructions](https://github.com/Luc4G3r/WSL2_dev_setup/blob/main/systemd/SYSTEMD_SETUP.md) as it is required to run phpstorm from terminal
* Install PHPStorm  
  `snap install phpstorm --classic`
* To verify, run  
  `phpstorm`
  This should open PHPStorm on Windows XServer

[More details](https://github.com/lackovic/notes/tree/master/Windows/Windows%20Subsystem%20for%20Linux#run-a-linux-gui-application-in-wsl-2)

## Or install PHPStorm on Windows
Versions >= 2020.1.3 support WSL2 path, but indexing is VERY slow.  
[Installation Instructions](https://www.jetbrains.com/help/phpstorm/installation-guide.html#standalone)

## Docker
[Show Docker (WSL2) Setup instructions](https://github.com/Luc4G3r/WSL2_dev_setup/blob/main/Docker/DOCKER_SETUP.md)

## DDEV
[Show DDEV Setup Instructions](https://github.com/Luc4G3r/WSL2_dev_setup/blob/main/Docker/DDEV_SETUP.md)

## Docker Magneto 2 WSL 2 Setup instructions
Work in progress.
[Show instructions](https://github.com/Luc4G3r/WSL2_dev_setup/blob/main/Docker/DOCKER_MAGENTO2_SETUP.md)

# More information
* Due to the architecture of WSL2, you don't want your projects being located in the windows file system (as WSL2 is slower than WSL here)
* Instead, you create and manage your projects inside the subsystem and use Visual Studio Code (great WSL2 support Plugins available, more stable than PHPStorm right now) or any other editor software (f.e. PHPStorm inside Linux with XServer desktop connected on Windows) to edit them
