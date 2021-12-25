# WSL2_dev_setup
Setup instructions WSL2 for web development

# Prerequisites
For this setup to work, you need to...
* ...have either Windows 11 or Windows 10 version 2004 and higher (Build 19041 and higher) installed
* ...have CPU virtualization feature turned on in BIOS
* ...have Hyper V Windows Service activated
 ```
  dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
 ```
 ...in Powershell does the job.  

# Setup
## Goals:
  * Running ddev webserver managed with [Docker Desktop](https://www.docker.com/products/docker-desktop) for all services your webproject requires
  * Running PHPStorm on Windows host machine, including container management (via hook to Docker Desktop), fast file system indexation, WSL2 terminal support and much more
## Reasons:
  * Virtual Machine Performance issues
  * Dual desktops and switching between host machine and virtual machine disturbs workflows
  * Full webserver scope management can be extremely tedious
  * Breaking one service can destroy your whole webserver setup

**_NOTE: This instruction manual is still work in progress, as is WSL2. I take no responsibility for any damage done to your system._**
## WSL
### Installation
[Microsoft's instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10)  
[Microsoft's instructions (GER)](https://docs.microsoft.com/de-de/windows/wsl/install-win10)
* In Windows Powershell, run:
```
 wsl --install
 wsl --set-default-version 2
```
### Configuration
* In Windows User directory, create or update `.wslconfig`:  
```
 [wsl2]
 memory=10GB
 processors=8
 swap=10GB
 localhostForwarding=true
```
  Change these values according to your needs / specs
### Ubuntu 20 LTS installation
* To install Ubuntu 20 LTS or any other of the supported distributions open [Windows Store](https://aka.ms/wslstore) search for it, and run the installation
* Launch your distribution!
## PHPStorm on Windows
[Installation Instructions](https://www.jetbrains.com/help/phpstorm/installation-guide.html#standalone)  

## Docker
[Show Docker Desktop (WSL2) Setup instructions](https://github.com/Luc4G3r/WSL2_dev_setup/blob/main/Docker/DOCKER_SETUP.md)

## DDEV
[Show DDEV Setup Instructions](https://ddev.readthedocs.io/en/stable/)
* Natively supports Magento 2 projects!

# More information
* Due to the architecture of WSL2, you don't want your projects being located in the Windows file system (as WSL2 is significantly slower here, due to file system difference)
* Instead, you create and manage your projects inside the subsystem and use any IDE on the host Windows system to edit them
