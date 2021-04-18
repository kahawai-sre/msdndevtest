:construction:
# msdndevtest
Review steps for activating an MSDN Enterprise subscription and setting up a WSL2, Ubuntu, Docker Desktop, and VSCode development environment for Microsoft Azure
# Prep Azure and Office 365 environment
## Activate and prep Azure and M365 E5 Developer MSDN Enterprise benefits
1. Assuming your MSDN Enterprise License has been assigned to a corporate account, log on to my.visualstudio.com as e.g. corpuser@corpdomain.com
2. Select the "Azure Subscription" benefit and hit Activate
3. You will be prompted to select an account to map the Azure subscription to. Sign in with your personal account e.g. myuser@hotmail.com.
4. Signed in to my.visualstudio.com as corpuser@corpdomain.com again, and activate the "Microsoft 365 - Developer Subscription (E5)" benefit. 
5. When prompted, select the "Setup Subscription option to create as a __NEW__ tenant and create a new "onmicresoft.com" subdomain and admin user when prompted e.g. myadmin@mynewdomain.onmicrosoft.com):
![](/img/M365E5.jpg "Select new M365 subscription")
6. Log on to the __Azure Portal__ as the new M365 E5 admin user e.g. myadmin@mynewdomain.onmicrosoft.com, and add the personal account used for the MSDN Azure Subscrption benefit (e.g. myuser@hotmail.com) as a Global Admin. There will be no subscriptions visible at this point.
7. Log on to the __Azure Portal__ as the user who was assigned the new Azure Subscription (e.g. myuser@hotmail.com), and grant the new user created when activating the M365 E5 benefit (e.g. myadmin@mynewdomain.onmicrosoft.com) as a Global Admin.
8. Still signed in to the Azure Portal as __myuser@hotmail.com__, under Subscriptions, select the new msdn subscription created when activating the Azure Subscription Benefit. In the OverView page select "Change Directory" and select the new directory created for the M365 E5 benefit i.e. mynewdomain.onmicrosoft.com
9. Sign in to the Azure Portal as myadmin@mynewdomain.onmicrosoft.com. After some time the new MSDN-SKU Azure Subscription will visible and ready for use under this account.
10. The net result is you have a fully functional Azure / Azure AD environment with E5 licensing for use testing Enterprise features like Conditional Access, Azure Identitiy Security, Azure PIM etc.
## Prep AAD in the new tenant (optional)
1.  Sign in to the Azure Portal as myadmin@mynewdomain.onmicrosoft.com
2.  In the Azure Active Directory service view, optionally configure:
  - Custom domains names (if you own a domain and want to use for AAD testing, rather than the default "onmicrosoft.com" sub-domain)
  - Company branding
  - Change Security Defaults to use Conditional Access for testing
  - Create test users
  - Configure AD Connect (requires a test VM running a traditional Active Directory domain, with connectivity to the Internet)
  - ....
## Assign E5 developer licenses to AAD users in the new tenant, and activate PowerAutomate Per User trial license (for testing PowerAutomate with Enterprise features)
1.  Sign in to the Office 365 admin center https://admin.microsoft.com/Adminportal/Home#/homepage as your MSDN M365 E5 tenant admin i.e. myadmin@mynewdomain.onmicrosoft.com
2.  Select Billing and then Purchase Services
3.  Find the "Power Auotmate per user service", and sign up for a trial
4.  Sign in to the Azure Portal as myadmin@mynewdomain.onmicrosoft.com
5.  In the Azure Active Directory service view, select users, select the user to confgiure licensing for, select "Licenses" and add an assignment for E5 and Powerapps as required.
## Activate your MSDN Enterprise Azure Devops instance and connect to the new AAD tenant:
1. Log on to the Azure Portal as a Global Admin
2. Add a new user, and select "Invite guest"
3. Enter yor corporate account email address e.g. corpuser@corpdomain.com
4. Under Roles, add the corpuser@corpdomain.com guest user as a "Azure DevOps Administrator" (may need GLobal Admin?)
5. Make sure the guest user accepts the invite and is added as a user to the AAD instance
6. Sign in to my.visualstudio.com as your corporate user account (the one granted MSDN Enterprise benefits)
7. Next to the "Azure Devops" link, click on "Get Started"
8. When prompted, select to join an __existing tenant__ (not the corporate one, although it can be changed later). 
Connecting an Azure Devops org to AAD provides some centralised Authentication and RBAC capabilities that are otherwise not available.
10. From the list, you should see the AAD tenant that the corpuser@corpdomain.com was granted Azure Devops Administrator (Global Amdin??) on.
11. Select that. Azure Devops will load with your corp user corpuser@corpdomain.com as the Organization admin.
12. Under Organisation Settings => Users, add other users e.g. myadmin@mynewdomain.onmicrosoft.com
13. Grant the new users an ADO RBAC role(s)
14. Optionally change the ADO organisation owner 
15. Optionally enable Azure Devops for AAD Conditional Access integration (Under Security => Policies). __Ensure there is an AAD CA policy with a rule allowing your access before you do this!__
# Prep local dev-test environment - Assumes Windows 10 Version 1903 or higher, with Build 18362 or higher
## Install Windows Subsystem for Linux v2 -WSL2 (on Windows host)
Full details here: https://docs.microsoft.com/en-us/windows/wsl/install-win10
1. Enable subsystem for Linux:
  ```
  dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
  ```
2. Enable Virtual Machines (not full Hyper-V)
  ```
  dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
  ```
4. Download and installl the WSL2 Linux Kernel package: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
5. Reboot
## Install Ubuntu as a WSL2 instance (on Windows host)
### Option 1: From the Microsoft Store
1. Open Micrsosoft Store, for Ubuntu 20:04 use this link https://www.microsoft.com/store/apps/9n6svws3rx71
2. Follow the normal Store options to install
### Option 2: From the Ubuntu Cloud Images library:
1. From https://cloud-images.ubuntu.com/releases navigate to the desired release folder e.g. Ubuntu 20.10 https://cloud-images.ubuntu.com/releases/groovy/release/
2. Find the file with suffix ..."-amd64-wsl.rootfs.tar.gz" and download e.g. https://cloud-images.ubuntu.com/releases/groovy/release/ubuntu-20.10-server-cloudimg-amd64-wsl.rootfs.tar.gz
3. Copy the downloaded install to a subfolder under your home directory e.g. C:\Users\usera\wsl-distros
4. From a command prompt, run the following to create a new WSL instance:
        ```wsl.exe --import <unique-name-for-distro> <path_to_target_install_folder> <path_to_-amd64-wsl.rootfs.tar.gz _file> ``` e.g.
  ```
  wsl.exe --import Ubuntu-20.10 C:\Users\usera\wsl-distros C:\Users\justinturver\Downloads\ubuntu-20.10-server-cloudimg-amd64-wsl.rootfs.tar.gz
  ```
        Note that this will create a .vhdx image in the target install folder for the distro
6. Repeat steps for additional Ubuntu distros or alternate Linux images
## Run first boot for the new WSL instance - if installed via __Windows Store__ (on Windows host)
 1. From a Windows __command prompt__, run the following to get a shell on the new WSL instance:
     ``` wsl.exe --distribution "<name_of_new_distro>" ```
3. When prompted, enter a new username and password.
4. __Remember the password__, it will be required for sudo from here on
5. Once you have the WSL shell up, run the following to update the Ubuntu core and packages:
    ```
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    sudo apt-get autoremove
    ```
## Run first boot for the new WSL instance - if installed via importing a WSL Cloud image using __wsl.exe --import ..__ (on Windows host)
1. When creating a new Ubuntu or other WSL image via the command line, vs. Windows Store, you will need to create a new non-root user, configure for sudo, and set as the default user when opening a WSL shell for the distro.
   1. From a Windows __command prompt__, run the following to get a shell on the new WSL instance:
       ``` wsl.exe --distribution "<name_of_new_distro>" ```
        ![](/img/startwsl.jpg "Select new Ubuntu distro")
   2. Create a new Ubuntu user and set password when prompted:
      ``` 
      sudo adduser jtadmin
      ```
   3. Set the new user as the default (vs root) when connecting to the WSL distro by adding the the [user] section to /etc/wsl.conf as per below:
      ```
      cat <<EOF >/etc/wsl.conf
      [user]
      default=jtadmin
      EOF
      ```
   4. Add the new user to the sudo group:
      ```
      sudo usermod -aG sudo jtadmin
      ```
   5. Type "exit" to get back to the windows command prompt
   6. Terminate the new instance so the new user setting scan take effect on next start.
      ```
      wsl.exe --terminate Ubuntu-20.10
      ```
   7. Open the new distro again and verify your shell is now in the context of the new user:
      ``` 
      wsl.exe -- distribution Ubuntu-20.10
      ```
2. Update the Ubuntu distro as follows:
    ```
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    sudo apt-get autoremove
    ```

## Install Cascadia Code PL font (Windows host):
1. Download https://github.com/microsoft/cascadia-code/releases/download/v2102.25/CascadiaCode-2102.25.zip (or newer from https://github.com/microsoft/cascadia-code/releases)
2. Extract the /ttf/CascadiaCodePL.ttf font
3. In Windows, Start => Settings => Search for "Font Settings" and open
4. Drag the extracted .ttf file to the "Add fonts" control in the4 FOnt Settings window.

## Install and configure Windows Terminal (Windows host)
Windows Terminal is a highly responsive terminal for managing Powershell, CLoudShell and WSL sessions
![](/img/distro.jpg "Select new Ubuntu distro")
1. Search for and install "Windows Terminal" from the Windows Store
2. Windows Terminal uses a "settings.json" configuration file to customise the distros and settings for each (appearance etc)
3. Open Windows Terminal, and select Settings from the drop down in the window navigation bar
4. Within the settings.json file, under "profiles" all registered WSL distros are visible. Any new distro will be registered in here automatically if either of the methods above are used (Windows Store or wsl --import)
5. There are a lot of options available for customisation - see https://docs.microsoft.com/en-us/windows/terminal/customize-settings/startup. Suggest the ones below as a start:
    1. Set the "defaultProfile" to the GUID of the distro you want as the default when the a new terminal session is opened:
      ```
      {
        "$schema": "https://aka.ms/terminal-profiles-schema",
        "defaultProfile": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
        ...
      }
      ```
    2. Update WSL profiles to set the home diretory to the native Linux filesystem path (defaults to /mnt/c/users/<username> which has poor performance)
      ```
      {
          "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
          "hidden": false,
          "name": "Ubuntu-20.10",
          "source": "Windows.Terminal.Wsl",
          "startingDirectory": "\\\\wsl$\\Ubuntu-20.10\\home\\jtadmin",
          "fontFace" : "Cascadia Code PL",
          "fontSize": 10
      }
      ```
    3. Update the Powershell profile to optimise appearance:
      ```
      {
        "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
        "name": "Windows PowerShell",
        "commandline": "powershell.exe",
        "hidden": false,
        "fontSize": 10,
        "acrylicOpacity": 0.7,
        "colorScheme" : "Frost",
        "cursorColor" : "#000000",
        "fontFace" : "Cascadia Code PL",
        "useAcrylic": true
      },
      ```
    4. Define the "Frost" colour scheme for Powershell
        ```
        "schemes": [
            {
                "name" : "Frost",
                "background" : "#FFFFFF",
                "black" : "#3C5712",
                "blue" : "#17b2ff",
                "brightBlack" : "#749B36",
                "brightBlue" : "#27B2F6",
                "brightCyan" : "#13A8C0",
                "brightGreen" : "#89AF50",
                "brightPurple" : "#F2A20A",
                "brightRed" : "#F49B36",
                "brightWhite" : "#741274",
                "brightYellow" : "#991070",
                "cyan" : "#3C96A6",
                "foreground" : "#000000",
                "green" : "#6AAE08",
                "purple" : "#991070",
                "red" : "#8D0C0C",
                "white" : "#6E386E",
                "yellow" : "#991070"
            }
        ],
        ```
    4. The complete settings.json file should look like this:
      ```
        {
          "$schema": "https://aka.ms/terminal-profiles-schema",
          "defaultProfile": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
          "copyOnSelect": false,
          "copyFormatting": false,
          "profiles":
          {
              "defaults":
              {
                  // Put settings here that you want to apply to all profiles.
              },
              "list":
              [
                  {
                      "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                      "name": "Windows PowerShell",
                      "commandline": "powershell.exe",
                      "hidden": false,
                      "fontSize": 10,
                      "acrylicOpacity": 0.7,
                      "colorScheme" : "Frost",
                      "cursorColor" : "#000000",
                      "fontFace" : "Cascadia Code PL",
                      "useAcrylic": true
                  },
                  {
                      "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                      "name": "Command Prompt",
                      "commandline": "cmd.exe",
                      "hidden": false
                  },
                  {
                      "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
                      "hidden": false,
                      "name": "Ubuntu-20.04",
                      "source": "Windows.Terminal.Wsl",
                      "startingDirectory": "\\\\wsl$\\Ubuntu-20.04\\home\\jtadmin",
                      "fontFace" : "Cascadia Code PL",
                      "fontSize": 10
                  },
                  {
                      "guid": "{3d0f99a1-1396-5f59-a7d5-39ae93daa8d5}",
                      "hidden": false,
                      "name": "Ubuntu-20.10",
                      "source": "Windows.Terminal.Wsl",
                      "startingDirectory": "\\\\wsl$\\Ubuntu-20.04\\home\\jtadmin",
                      "fontFace" : "Cascadia Code PL",
                      "fontSize": 10
                  },
                  {
                      "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                      "hidden": false,
                      "name": "Azure Cloud Shell",
                      "source": "Windows.Terminal.Azure"
                  }
              ]
          },
          "schemes": [
              {
                  "name" : "Frost",
                  "background" : "#FFFFFF",
                  "black" : "#3C5712",
                  "blue" : "#17b2ff",
                  "brightBlack" : "#749B36",
                  "brightBlue" : "#27B2F6",
                  "brightCyan" : "#13A8C0",
                  "brightGreen" : "#89AF50",
                  "brightPurple" : "#F2A20A",
                  "brightRed" : "#F49B36",
                  "brightWhite" : "#741274",
                  "brightYellow" : "#991070",
                  "cyan" : "#3C96A6",
                  "foreground" : "#000000",
                  "green" : "#6AAE08",
                  "purple" : "#991070",
                  "red" : "#8D0C0C",
                  "white" : "#6E386E",
                  "yellow" : "#991070"
              }
          ],
          "actions":
          [
              { "command": {"action": "copy", "singleLine": false }, "keys": "ctrl+c" },
              { "command": "paste", "keys": "ctrl+v" },
              { "command": "find", "keys": "ctrl+shift+f" },
              { "command": { "action": "splitPane", "split": "auto", "splitMode": "duplicate" }, "keys": "alt+shift+d" }
          ]
      }
    ```

## Configure the WSL bash theme (Windows host then WSL shell):
1. Open Windows Terminal and if not the default, open a new Ubuntu-20.xx shell/window
2. Run the following commands to install powerline-go
    ```
    sudo apt install golang-go
    go get -u github.com/justjanne/powerline-go
    ```
3. Add the following to __~/.bashrc__ (sudo vim ~/.bashrc):
    ```
    GOPATH=$HOME/go
    function _update_ps1() {
        PS1="$($GOPATH/bin/powerline-go -error $?)"
    }
    if [ "$TERM" != "linux" ] && [ -f "$GOPATH/bin/powerline-go" ]; then
        PROMPT_COMMAND="_update_ps1; $PROMPT_COMMAND"
    fi
    ```

## Install or update Python3 and pip3 (WSL shell)
Ubuntu 18.04 or 20.04 only - ships with 20.10+ by default
1. Open the new WSL instance in Windows Terminal
2. Run the following to install python 3.8
    ```
    sudo apt update
    sudo apt install software-properties-common
    # OPTIONAL: sudo add-apt-repository ppa:deadsnakes/ppa (this will specify an upstream repo for python versions but will move out of step/support of Ubuntu default)
    sudo apt install python3.8
    python3 --version
    ```
3. Run the folling to install pip3 (python package manager)
    ```
    sudo apt update
    sudo apt install python3-pip
    sudo pip3 install --upgrade pip
    ```
## Install Azure CLI (WSL Shell):
1. Open the WSL distro shell in Windows Terminal
2. Run the following command to execute the MS maintained Linux install script. THis will install and configure the latest version:
    ```
    sudo curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
    ```
2. Configure Azure cli to auto install extensions:
    ```
    az config set extension.use_dynamic_install=yes_prompt
    az config set extension.run_after_dynamic_install=yes
    ```
3. Optional - pre-install desired extensions:
    ```
    az extension add --name aks-preview
    az extension add --name connectedk8s
    az extension add --name k8sconfiguration
    az extension add --name azure-firewall
    az extension add --name azure-devops
    az extension add --name cosmosdb-preview
    ..etc
    ```
## Install Powershell core (WSL shell)
1. Open the WSL distro shell in Windows Terminal, and run the following:
  ```
  # Update the list of packages
  sudo apt-get update
  # Install pre-requisite packages.
  sudo apt-get install -y wget apt-transport-https software-properties-common
  # Download the Microsoft repository GPG keys
  wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
  # Register the Microsoft repository GPG keys
  sudo dpkg -i packages-microsoft-prod.deb
  # Update the list of products
  sudo apt-get update
  # Enable the "universe" repositories
  sudo add-apt-repository universe
  # Install PowerShell
  sudo apt-get install -y powershell
  # Start PowerShell to test
  pwsh
  ```
2. Install modules - e.g. Az
  ```
  Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
  ```
## Install Terraform binary (WSL shell):
1. Open the WSL distro shell in Windows Terminal
2. Download latest version of Linux x64, and unzip into path:
  ```
  sudo apt install unzip
  curl https://releases.hashicorp.com/terraform/0.15.0/terraform_0.15.0_linux_amd64.zip --output terraform-latest.zip
  sudo unzip terraform-latest.zip
  sudo cp terraform /usr/bin
  terraform -v
  ```
## Install and configure Git for Windows (Windows Host)
1. In Windows, download and run the 64-bit Git for Windows setup from here: https://git-scm.com/download/win
2. Install with the installation _defaults_
3. Note that at the time of writing, the bundled version of Git Credential Manager Core seems to have issues. Assuming that is still the case, proceed to the next section to install and the configure standalone Git Vredential Manager Core
## Install and configure Git Credential Manager __Core__ (Windows Host and WSL)
Tested version at this time is __v2.0.289__. For other releases go to https://github.com/microsoft/Git-Credential-Manager-Core/releases
1. In Windows, download and install https://github.com/microsoft/Git-Credential-Manager-Core/releases/tag/v2.0.289-beta
2. This will install into _C:\Program Files (x86)\Git Credential Manager Core_, where the binary _git-credential-manager-core.exe_ resides
3. We need to configure the WSL/Linux distro to reference that path as per steps below:
  1. In Windows Terminal, open a shell for Ubuntu-20.10 or the recently installed distro
  2. Run the following command to configure Linux to call the Git Credential Manager Core via the Windows installed path (either via Git for Windows install path with the net "Credential Manager Core" option selected, or via the standalone GIt Credential Manager Core install path:
      ```
      sudo -i
      file=/usr/bin/git-credential-manager
      echo 'exec /mnt/c/Program\ Files\ \(x86\)/Git\ Credential\ Manager\ Core/git-credential-manager-core.exe $@' > $file
      chmod +x $file
      exit
      ```
  3. The resulting script ```/usr/bin/git-credential-manager ``` should have single line as follows:
      ```exec /mnt/c/Program\ Files\ \(x86\)/Git\ Credential\ Manager\ Core/git-credential-manager-core.exe $@```
  5. Run the following command to configure git to call Git Credential Manaager Core:
      ```
      cat << EOF >> ~/.gitconfig
      [credential]
      helper = manager
      EOF
      ```
  4. Run ```git config -l``` and verify the output shows this line:
      ```credential.helper=manager```
  6. No other git config is necessary
  7. Git Credential Manager Core will call an OAuth flow against either github or Azure Devops repos depending on the source/target of the git operation:
      ![](/img/aadauth.jpg "Example GCMC auth flow")
## Install and configure VSCode (Windows Host and WSL)
1. In Windows, browse to https://code.visualstudio.com/ and download and run the VSCode installer
2. When prompted, make sure the following is selected:
   ![](/img/add_to_path.png "Add to path option")
3. Open Vistual Studio Code, and install the "Remote WSL" extension:
   ![](/img/Remote.JPG "Install Remote WSL Extension")
4. From Windows Terminal open a new WSL session
5. Navigate to any folder (e.g. with some code), and run 
    ``` code . ``` 
6. The first time this is executed, the VS code server for WSL will be installed as shown below
     ![](/img/Code.JPG "Initialise VSCode in WSL")
7. Subsequent runs of ```code .``` will launch VSCode in the WSL context
8. When VSCode is launched in the WSL context, installing VSCode extensions will prompt to install in WSL and - if relevant - Windows contexts.
## Install Docker Desktop (Windows Host)
1. To get a functional docker daemon that can be called by both Windows and WSL clients, from your Windows host, download and execute Docker Desktop from https://docs.docker.com/docker-for-windows/install/#install-docker-desktop-on-windows
2. Ensure you have at least version 3.2.2.x or above 
3. When installing, make sure you select __"Install required Windows components for WSL 2"__ ... do _NOT_ select the Hyper-V option
4. After installation, start Docker Desktop (if not already started)
5. From the taskbar, Open the Docker Desktop GUI
6. Click the settings button at the top right
7. Navigate to "Resources"
8. Select the WSL distros you want to allow to connect to the Docker daemon:
    ![](/img/DockerDesktop.JPG "Enable distros for Docker")
10. Open a Windows powershell or command prompt, and run the following command which shows that Docker Desktop is running as a WSL 2 distro, not a VM: 
    ```wsl --list -v```:
    ![](/img/wslilist.jpg "List wsl distros")
## Install the Docker client in WSL
1. Open a WSL shell for your distro
2. Install docker:
  ```sudo apt install docker.io```
4. Verify WSL can talk to the Docker Daemon in Docker Desktop:
  1. Open the WSL distro shell in Windows Terminal
  2. Run ```docker ps``` and verify there are no errors
  3. Run ```docker container run hello-world``` and verify "hello world" is output when the container runs
## Configure the Powershell "Frost" theme  for Windows Terminal (Windows host)
1. Open __Windows Terminal__
2. Click the Drop Down arrow and select a new __Powershell__ window
3. Install:
  ```
  Install-Module oh-my-posh -Scope CurrentUser
  ```
4. From the powershell prompt in Windows Terminal, run:
  ```
  notepad $PROFILE
  ```
5. Enter the lines shown below and __save the file__:
  ```
  Import-Module posh-git
  Import-Module oh-my-posh
  Set-PoshPrompt Paradox
  ```
  
## Useful links
* https://wiki.ubuntu.com/WSL




