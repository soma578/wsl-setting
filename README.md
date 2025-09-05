# WSL (Windows Subsystem for Linux) Setup Guide

This guide provides step-by-step instructions for setting up the Windows Subsystem for Linux (WSL) on your Windows machine.

## 1. System Requirements

- Windows 10 (Version 1607 or later) or Windows 11.
- Virtualization must be enabled in your computer's BIOS/firmware settings.

## 2. Installation

The easiest way to install WSL is with a single command.

1.  **Open PowerShell as Administrator**:
    - Click the Start menu, type "PowerShell".
    - Right-click on it and select "Run as administrator".

2.  **Run the Install Command**:
    - In the PowerShell window, execute the following command:
      ```bash
      wsl --install
      ```

3.  **Restart Your Computer**:
    - After the process is complete, restart your machine.

This command will automatically:
- Enable the necessary Windows features (like the Virtual Machine Platform).
- Download the latest Linux kernel.
- Install Ubuntu as the default Linux distribution.

## 3. Initial Linux Setup

After your computer restarts, the installation will continue.

1.  **Launch Linux**: The first time you launch your new Linux distribution, a console window will open and wait for a few moments for the files to decompress.
2.  **Create a User Account**: You will be prompted to create a username and password for your Linux distribution. These credentials do not need to match your Windows login information.

## 4. Installing a Different Linux Distribution (Optional)

If you want to install a different Linux distribution:

1.  **List Available Distributions**:
    ```bash
    wsl --list --online
    ```

2.  **Install a Specific Distribution**:
    ```bash
    wsl --install -d <DistroName>
    ```
    (Replace `<DistroName>` with the name from the list).

## 5. Using WSL

You can access your Linux distribution by:
- Launching it from the Start Menu (e.g., "Ubuntu").
- Typing `wsl` or `ubuntu` in PowerShell or Command Prompt.

Your Windows file system is accessible from within WSL under the `/mnt/` directory (e.g., `cd /mnt/c/Users/YourUser`).

## 6. Pushing to GitHub

To share this guide or your WSL projects on GitHub:

1.  **Create a new repository** on [GitHub](https://github.com/new).
2.  **Initialize Git** in your local `wsl-setting` directory:
    ```bash
    git init
    git add .
    git commit -m "Initial commit: Add WSL setup guide"
    ```
3.  **Link the remote repository and push**:
    ```bash
    git remote add origin https://github.com/YourUsername/YourRepoName.git
    git branch -M main
    git push -u origin main
    ```
    (Replace `YourUsername` and `YourRepoName` with your actual GitHub details).
