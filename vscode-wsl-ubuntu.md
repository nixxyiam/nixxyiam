# Setup VS Code for Markdowns on WSL2 Ubuntu

This guide explains how to set up VS Code to modify Markdown files on WSL2 Ubuntu LTS with Visual Studio Code as the your main IDE.
This guidewas validated on Windows 11 Home 23H2.

## Setup VS Code and WSL2 Ubuntu on Windows 11

### Software Pre-requisites in Microsoft Store

Ensure that you have the following components installed from Microsoft Store:

1. Windows Subsystem for Linux, Microsoft Corporation
2. Ubuntu 22.04.2 LTS, Canonical Group Limited
3. Visual Studio Code, Microsoft Corporation
4. Windows Terminal, Microsoft Corporation

### Extension Pre-requisites in Visual Studio Code Marketplace

Ensure that you have the following extensions installed from [VS Code Marketplace](https://code.visualstudio.com/docs/editor/extension-marketplace):

1. Remote Development, Microsoft. This is needed for VS Code to communicate with WSL2 Ubuntu.
2. (optional) markdownlint, David Anson. A useful linter for Markdown files.

## Initialise WSL2 Ubuntu 

Once you have completed installing the pre-requisite software and VS Code extensions, we will configure Ubuntu to open VS Code.

### Configuring Ubuntu for the first time

1. Go to Terminal -> Ubuntu 22.04.2 LTS to open Ubuntu terminal.
2. This will trigger Ubuntu to perform an initial setup. After which, you will be asked to create a set of username and password for Ubuntu.
3. After creating your username and password, update Ubuntu with the following:

        sudo apt-get update
        sudo apt-get upgrade

### Configuring Ubuntu Communication with VS Code

1. Ensure you are in Ubuntu terminal.
2. Create a dummy markdown file with `echo '# Test Markdown File' > test.md`.
3. Get Ubuntu to ask VS Code to open the dummy markdown file with `code ./test.md`.
4. Ubuntu will perform a one-time installation for VS Code Server. After which, VS Code will pop up with test.md file opened.

## Useful Preview Feature in VS Code

As VS Code supports Markdown out of the box, it has a nifty feature to see the Markdown preview side-by-side with the
Markdown source. To bring up the side-by-side preview window, ensure your cursor is within the Markdown source file and 
press `Ctrl+K V`. Alternatively, you can press `Ctrl+Shift+P` and search for "Markdown: Preview to the Side".

If you wish to learn more about Markdown features in VS Code, you can check out the official 
[Microsoft resource](https://code.visualstudio.com/docs/languages/markdown).

