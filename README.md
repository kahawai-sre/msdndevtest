# msdndevtest
Review steps for activating an MSDN Enterprise subscription and setting up a WSL2, Ubuntu and VSCode development environment for Microsoft Azure

## Activate MSDN Enterprise benefits (high level)

1. Assuming your MSDN Enterprise License has been assigned to a corporate account, log on to my.visualstudio.com as e.g. corpuser@corpdomain.com
2. Select the "Azure Subscription" benefit and hit Activate
3. You will be prompted to select an account to map the Azure subscription to. Sign in with your personal account e.g. myuser@hotmail.com.
4. Signed in to my.visualstudio.com as corpuser@corpdomain.com, activate the "M365 E5" benefit. When prompted, select the option to create as a __NEW__ tenant and create a new admin user when prompted e.g. myadmin@mynewdomain.onmicrosoft.com)
6. Log on to the Azure Portal as the new M365 E5 admin user, and add the personal account used for the MSDN Azure Subscrption benefit as a Global Admin. There will be no subscriptions visible at this point.
7. Add the y
8.	Add jtadmin@kahawai.onmicrosoft.com as Global Admin in directory used by jturver@hotmail.com
9.	Sign in to Azure Portal as jturver@hotmail.com, and transfer sub to directory for kahawai.onmicrosoft.com


## Install WSL 10
1. dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
2. dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
3. Download and installl the WSL2 Linux Kernel package: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

- Full details here: https://docs.microsoft.com/en-us/windows/wsl/install-win10


