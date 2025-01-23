# Running Arch on Windows via WSL - Setting up my Dev Environment  

Running Linux on Windows via the Windows Subsystem for Linux (WSL) offers developers a blend of **advantages from both platforms**.  

**Sources**:
- https://dev.to/jrcharney/archwsl-brings-arch-linux-to-wsl-and-more-1jm4
- https://github.com/jrcharney/ArchWSLPlusPlus

---

## Installing Arch Linux on WSL

While most users install Ubuntu, you can set up **any Linux distribution** you like, including Arch.  

### Enable Hyper-V

- If you're running Windows 11, open the Windows terminal as admin.
- Enable Hyper-V via
  - `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All`
- reboot

### Get a Windows Installer (Scoop)

You should install **Scoop** so you can download some stuff in the CLI, especially the **NerdFonts** that are required for Powerline, Powerlevel10k, and VS Code. These fonts have extra characters that can be used in the Windows Terminal.  

Having an installer like Scoop simplifies software management on Windows by allowing easy installation and updates of various applications directly from the command line.

I recommend Scoop (scoop) over Chocolatey (choco) because it is better organized. **Packages** are grouped into "**Buckets**".  

- Ensure that you have PowerShell 5.1 or later installed on your system:
  - `$PSVersionTable`
- set the PowerShell execution policy to allow script execution:
  - `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
- Execute the following command to download and install Scoop:
  - `irm get.scoop.sh | iex`
  - This command uses **Invoke-RestMethod** (aliased as irm) to fetch the installation script from the Scoop website and execute it.
- you can verify that Scoop is installed by running
  - `scoop -h`
- You can start installing packages using the command:
  - `scoop install <package-name>`

>[!note]
>Scoop automatically adds its installation directory (typically C:\Users\<USERNAME>\scoop\shims) to your system's PATH variable, allowing you to run installed applications from any CLI.


---
EOF
