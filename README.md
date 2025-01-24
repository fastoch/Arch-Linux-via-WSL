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
- Go to 'Windows Features' and enable 'Windows Subsystem for Linux'
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

---

## Getting Arch up and running

Start by running the Arch.exe program:
```powershell
arch
arch
```
We need to run it twice because the first time is for installing it.  

---
>[!note]
>To exit **bash** (the default Linux shell) and go back to **powershell**, simply run `exit`
---

Then, let's set our root password:
```bash
[root@hostname ~]# passwd
```

Once you have set up your root password, let's create our regular user and give them permission to use the sudo command.  
That command temporarily gives the user the power to run root command with the `sudo` prefix.

Create the sudoers file:
```bash
[root@hostname ~]# echo "%wheel ALL=(ALL) ALL" > /etc/sudoers.d/wheel	
```
The above command creates a new file (wheel) in the /etc/sudoers.d directory that grants members of the wheel group the ability to execute any command as any user on the system. 

---

Create a new user and make it a sudoer:
```bash
[root@hostname ~]# useradd -m -G wheel -s /bin/bash username
```
The `-m` option tells the system to create a home directory for the new user if it does not already exist.  
The `-G wheel` is for adding the user to the wheel group, which makes it a sudoer.  
The `-s /bin/bash` is for setting bash as the user's default shell.

Set the password for the new user
```bash
[root@hostname ~]# passwd fastoch
```

Return to powershell:     
```bash
[root@hostname ~]# exit
```								

We need to return to the Powershell for a moment to set the default user.
```powershell
arch config --default-user username					# Note: Use your username not mine!
```

Once we have our default user, we can launch Arch again.  
Thanks to how ArchWSL is set up, Arch will be an option in our dropdown menu on the Windows Terminal, but we can set it as the default option via the Terminal settings.  

The next time we start our Linux terminal, we should see something like this as our user prompt:
```bash
[username@hostname ~]$
```

---

## Setting up our custom Arch Linux

Here are the first commands you should issue:
```bash
sudo pacman-key --init
sudo pacman-key --populate
```
The first line initializes the GnuPG keyring for pacman, which is essential for managing and verifying the authenticity of packages.  
This is a crucial step in managing package signatures within the Arch Linux ecosystem.  

The second line fetches and installs the default GPG keys from the Arch Linux keyring, which are necessary for verifying package signatures.  
This ensures that the packages you install come from trusted sources and have not been tampered with during transit.  

---

Then we need to update the system by synchronizing the package database and upgrading all installed packages:
```bash
sudo pacman -Syu
```

---

After that, install the Arch Linux keyring, which is necessary to install ANY package from the Arch Linux repository:
```bash
sudo pacman -S archlinux-keyring
```

---

Let's install the base development tools + Git:
```bash
sudo pacman -S --needed base-devel git
```
When you install the `base-devel` package for ArchWSL, `fakeroot` and `fakeroot-tcp` are in conflict.  
When asked if you want to replace `fakeroot-tcp` with `fakeroot` SAY NO!

---

Create some placeholder directories:
```bash
mkdir Projects Downloads Documents Music Pictures Videos
```

mkdir bin                                                           # Add this directory. It should be added to your `$PATH`. You can use this directory to launch scripts that you write to do tasks
cd Downloads                                                        # Switch to the Downloads directory
git clone https://aur.archlinux.org/yay.git                                 # This will install `yay` which will allow you access to the ArchLinux User Repository.
cd yay                                                                      # Go to the yay directory (This should be in the Downloads directory)
makepkg -si                                                                 # Make the `yay` package, this will also install the Go Language (`golang`) that `yay` needs to do stuff.
yay --version                                                               # Verify that yay was installed.
rm -rf ~/yay                                                                # With yay successfully installed, you may remove the `yay` folder in your home directory. (This is optional)
cd ~
sudo pacman -S openssh github-cli pv fzf clang llvm rust python-pip lm_sensors psutils python-psutil neofetch htop bashtop imagemagick jq
```




---
EOF
