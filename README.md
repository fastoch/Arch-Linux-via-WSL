# Running Arch on Windows via WSL - Setting up my Dev Environment  

Running Linux on Windows via the Windows Subsystem for Linux (WSL) offers developers a blend of **advantages from both platforms**.  

**Sources**:
- https://dev.to/jrcharney/archwsl-brings-arch-linux-to-wsl-and-more-1jm4
- https://github.com/jrcharney/ArchWSLPlusPlus/blob/main/doc/00-START_HERE.md

While most users install Ubuntu, you can set up **any Linux distribution** you like, including **Arch**.  

---

## Prerequisites

- If you're running Windows 11, open the Windows terminal as admin.
- Enable Hyper-V via this command:
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```
- Go to Windows Features and enable Windows Subsystem for Linux
- reboot

---

## Get a Windows Software Installer (Scoop)

You should install **Scoop** so you can download some stuff in the CLI, especially the **NerdFonts** that are required for Powerline, Powerlevel10k, and VS Code. These fonts have extra characters that can be used in the Windows Terminal.  

Having an installer like Scoop simplifies software management on Windows by allowing easy installation and updates of various applications directly from the command line.

I recommend Scoop (scoop) over Chocolatey (choco) because it is better organized. **Packages** are grouped into "**Buckets**".  

- Open powershell as a regular user (not as admin)
- Ensure that you have PowerShell 5.1 or later installed on your system:
```powershell
$PSVersionTable
```
- check the current execution policy:
```powershell
Get-ExecutionPolicy
```
- if needed, set the PowerShell execution policy to allow script execution:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
- Execute the following command to download and install Scoop:
```powershell
irm get.scoop.sh | iex
```
This last command uses **Invoke-RestMethod** (aliased as irm) to fetch the installation script from the Scoop website and execute it.

- you can verify that Scoop is installed by running
```powershell
scoop -h
```
- You can update scoop via:
```powershell
scoop update
```
- You can start installing packages using the command:
```powershell
scoop install <package-name>
```

>[!note]
>Scoop automatically adds its installation directory (typically C:\Users\<USERNAME>\scoop\shims) to your system's PATH variable, allowing you to run installed applications from any CLI.

---

## Get the Nerd Fonts

Since the entire set of NerdFonts is about a couple of gigabytes, you should just get the essential fonts.  
I recommend installing these fonts from NerdFonts:
- Meslo
- Firacode
- Firamono
- Hack

We first need to add the nerd-fonts bucket:  
```powershell
scoop bucket add nerd-fonts
```

Next, install the font packages:  
```powershell
scoop install Meslo-NF Meslo-NF-Mono Hack-NF Hack-NF-Mono FiraCode-NF FiraCode-NF-Mono FiraMono-NF FiraMono-NF-Mono
```  

You should now be able to set your Windows terminal to use one of the fonts we've installed by going to the settings.  
A lot of folks are recommending using Meslo.

---

## Install ArchWSL

Before we run WSL for the first time, we need to set its version to version 2, and to update its kernel:
```powershell
wsl --set-default-version 2
wsl --update
```

To see the installed distributions and their WSL versions later on:
```powershell
wsl -l -v
```

Now we need to get ArchWSL from the extras bucket:
```powershell
scoop bucket add extras
scoop install archwsl
```

We are now ready to take care of some basic tasks to get Arch Linux up and running.

Start by running the Arch.exe program:
```powershell
arch.exe
arch.exe
```
We run it twice: one time for installing and second time for running it.

Then, lets set our root password:
```bash
[root@hostname ~]# passwd
```



---
EOF
