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
 -- Custom domains names (if you own a domain and want to use for AAD testing)
 -- Company branding
 -- Change Security Defaults to use Conditional Access for testing
 -- Create test users
 -- Configure AD Connect (requires a test VM running a traditional Active Directory domain, with connectivity to the Internet)
 -- ....

### Sign up for PowerApps Automate per user trial, and assign E5 licenses to AAD users in the new tenant
1.  Sign in to the Office 365 admin center https://admin.microsoft.com/Adminportal/Home#/homepage as your MSDN M365 E5 tenant admin i.e. myadmin@mynewdomain.onmicrosoft.com
2.  Select Billing and then Purchase Services
3.  Find the "Power Auotmate per user service", and sign up for a trial
4.  Sign in to the Azure Portal as myadmin@mynewdomain.onmicrosoft.com
5.  In the Azure Active Directory service view, select users, select the user to confgiure licensing for, select "Licenses" and add an assignment for E5 and Powerapps as required.

## Install WSL 10
1. dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
2. dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
3. Download and installl the WSL2 Linux Kernel package: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

- Full details here: https://docs.microsoft.com/en-us/windows/wsl/install-win10


