# msdndevtest
Review steps for activating an MSDN Enterprise subscription and setting up a WSL2, Ubuntu and VSCode development environment

## Install WSL 10
1.dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
2. dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
3. Download and installl the WSL2 Linux Kernel package: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

- Full details here: https://docs.microsoft.com/en-us/windows/wsl/install-win10


