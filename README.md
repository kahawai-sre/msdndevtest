# msdndevtest
Review steps for activating an MSDN Enterprise subscription and setting up a WSL2, Ubuntu and VSCode development environment for Microsoft Azure

## Prep Azure and Office 365 environment
### Activate ad prep Azure MSDN Enterprise benefits (high level, covers Azure Subscription and M365 E5 benefits only))

1. Assuming your MSDN Enterprise License has been assigned to a corporate account, log on to my.visualstudio.com as e.g. corpuser@corpdomain.com
2. Select the "Azure Subscription" benefit and hit Activate
3. You will be prompted to select an account to map the Azure subscription to. Sign in with your personal account e.g. myuser@hotmail.com.
4. Signed in to my.visualstudio.com as corpuser@corpdomain.com, activate the "M365 E5" benefit. When prompted, select the option to create as a __NEW__ tenant and create a new admin user when prompted e.g. myadmin@mynewdomain.onmicrosoft.com)
6. Log on to the Azure Portal as the new M365 E5 admin user e.g. myadmin@mynewdomain.onmicrosoft.com, and add the personal account used for the MSDN Azure Subscrption benefit (e.g. myuser@hotmail.com) as a Global Admin. There will be no subscriptions visible at this point.
7. Log on to the Azure Portal as the user who was assigned the new Azure Subscription (e.g. myuser@hotmail.com), and grant the new user created when activating the M365 E5 benefit (e.g. myadmin@mynewdomain.onmicrosoft.com) as a Global Admin.
8. Still signed in to the Azure Portal as myuser@hotmail.com, under Subscriptions, select the new MSDN-SKU subscription created when activating the Azure Subscription Benefit. In the OverView page select "Change Directory" and select the new directory created for the M365 E5 benefit i.e. mynewdomain.onmicrosoft.com
9. Sign in to the Azure Portal as myadmin@mynewdomain.onmicrosoft.com. After some time the new MSDN-SKU Azure Subscription will visible and ready for use under this account.

### Prep AAD in new tenant (optional)
1.  Sign in to the Azure Portal as myadmin@mynewdomain.onmicrosoft.com
2.  In the Azure Active Directory service view, look at configuring:
  - Custom domains names (if you own a domain and want to use for AAD testing)
  - Company branding
  - Change Security Defaults to use Conditional Access for testing
  - Create test users
  - Configure AD Connect (requires a test VM running a traditional Active Directory domain, with connectivity to the Internet)
  - ....

### Sign up for PowerApps Automate per user trial, and assign E5 licenses to AAD users in the new tenant
1.  Sign in to the Office 365 admin center https://admin.microsoft.com/Adminportal/Home#/homepage as your MSDN M365 E5 tenant admin i.e. myadmin@mynewdomain.onmicrosoft.com
2.  Select Billing and then Purchase Services
3.  Find the "Power Auotmate per user service", and sign up for a trial
4.  Sign in to the Azure Portal as myadmin@mynewdomain.onmicrosoft.com
5.  In the Azure Active Directory service view, select users, select the user to confgiure licensing for, select "Licenses" and add an assignment for E5 and Powerapps as required.

### Prep local dev-test environment - Assumes Windows 10 Version 1903 or higher, with Build 18362 or higher

## Install WSL 2
Full details here: https://docs.microsoft.com/en-us/windows/wsl/install-win10
1. Enable subsystem for Linux:
  >> dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
2. Enable Virtual Machines (not full Hyper-V)
  >> dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
4. Download and installl the WSL2 Linux Kernel package: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
5. Reboot

## Install Ubuntu (e.g. 20.04 LTS) as a WSL2 instance:
### Option 1: From the Microsoft Store
1. Open Micrsosoft Store, for Ubuntu 20:04 use this link https://www.microsoft.com/store/apps/9n6svws3rx71
2. Follow the options to install
### Option 2: From the Ubuntu Cloud Images library:
1. From https://cloud-images.ubuntu.com/releases navigate to the desired release folder e.g. 20.10 https://cloud-images.ubuntu.com/releases/groovy/release/
2. Find the file with suffix ..."-amd64-wsl.rootfs.tar.gz" and download e.g. https://cloud-images.ubuntu.com/releases/groovy/release/ubuntu-20.10-server-cloudimg-amd64-wsl.rootfs.tar.gz
3. Copy the downloaded install to a subfolder under your home directory e.g. C:\Users\usera\wsl-distros
4. From a command prompt, run the following to create a new WSL instance:
  >> wsl.exe --import <unique-name-for-distro> <path_to_target_install_folder> <path_to_-amd64-wsl.rootfs.tar.gz _file> e.g.
    `wsl.exe --import Ubuntu-20.10 C:\Users\usera\wsl-distros C:\Users\justinturver\Downloads\ubuntu-20.10-server-cloudimg-amd64-wsl.rootfs.tar.gz`
  >> Note that this will create a .vhdx image in the target install folder for the distro
6. Repeat steps for additional Ubuntu distros or alternate Linux images

## Install Windows Terminal
1. Search for and install "Windows Terminal" from the Windows Store
2. Once installed, Windows Terminal uses a "settings.json" configuration file to customise the distros and settings for each (appearance etc)
3. Open Windows Terminal, and select Settings from the drop down in the window navigation bar
4. Within the settings.json file, under "profiles" all registered WSL distros are visible. Any new distro will be registered in here automatically if either of the methods above are used (Windows Store or wsl --import)
5. There are a lot of optios available for customisation - see https://docs.microsoft.com/en-us/windows/terminal/customize-settings/startup
  >> Set the "defaultProfile" to the GUID of the distro you want as the default when the a new terminal session is opened
7. 

## Useful links
* https://wiki.ubuntu.com/WSL
* 



