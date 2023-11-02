-----------------------
# Setting Up Git Bash
-----------------------
Setting up Git Bash involves installing Git on your computer and configuring it to work with your identity and preferences. Here's a step-by-step guide on how to set up Git Bash on your computer:

## Step 1: Download Git Bash

1. Visit the official Git website: [Git Official Website](https://git-scm.com/).

2. Download the Git installer for your operating system (Windows, macOS, or Linux).

## Step 2: Install Git

1. Run the Git installer that you downloaded in the previous step.

2. Follow the installation wizard's instructions:
   - Accept the license agreement.
   - Choose the components to install (default selections are usually fine).
   - Choose an editor (the default is usually fine; you can change it later).
   - Choose the default branch name (usually "main" or "master").
   - Configure the PATH environment: Select "Use Git from Git Bash only" to avoid conflicts with other programs.
   - Click "Install" to complete the installation.

## Step 3: Verify Git Installation

To ensure that Git is installed correctly, run the following command in Git Bash:

```bash
git --version
```

This command should display the installed Git version.

## Step 4: Configure Git Identity

1. Open Git Bash, click on the Start button, type git, and select Git Bash.

2. Set your Git username and email by running these commands, replacing Your Name and your.email@example.com with your own information:

   ```bash
   git config --list
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

-------------------------------
# Install Git Client on Linux
--------------------------------
To work with Git on a Linux-based system, you can use your distribution's package manager to install the Git client. Follow the instructions below for your specific Linux distribution

## CentOS/RHEL:

```bash
sudo yum install git -y
```
** Note: with yes response **

After executing the appropriate command for your distribution, Git and its dependencies will be installed. To verify the installation, run:

```bash
git --version
```

This will display the installed Git version. If you see the Git version, it means Git has been successfully installed on your Linux system. You can now start using Git for version control and collaboration on your projects.

-------------------------
# Start Using Git 
-------------------------
You're now set up to use Git Bash for version control. You can create repositories, commit changes, and collaborate with others using Git commands.

Here's a simple example to create a Git repository:

1. Navigate to the directory where you want to create a Git repository in Git Bash.

2. Initialize a new Git repository:

   ```bash
   git init
   ```

You can now add files, make commits, and work with Git using Git Bash.

That's it! You've successfully set up Git Bash or Git client on Linux on your computer and can begin using Git for version control.
