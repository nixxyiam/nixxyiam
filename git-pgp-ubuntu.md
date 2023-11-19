# Easy Guide to Setup WSL2 Git with VS Code

This guide aims to minimise time for developers to set up new Windows
development systems that require Git, GitHub, GPG, and Visual Studio Code
(VS Code) as part of the development workflow.

The following assumptions have been made to avoid an overly lengthy write-up:

- Development system uses
  - Windows 11 Home 23H2 or later.
  - WSL2 Ubuntu 22.04.2 LTS.
- The developer has an existing GitHub account.
- The developer's GitHub account has at least one repository.
- Signed commits to GitHub is desired.
- VS Code will be the primary Git editor.

## Install Git

1. Open Ubuntu 22.04.2 LTS terminal. Ensure packages are updated.

        sudo apt-get update
        sudo apt-get upgrade

1. Install latest version of git.

        sudo apt-get install git-all
   Verify with

        git version

1. Set your git username and commit email address.

        git config --global user.name "Your Name"
        git config --global user.email "youremail@domain.com"

   Verify with

        git config --global --list

## Install GitHub CLI and connect to GitHub

1. Install GitHub CLI.

        sudo apt-get install gh

1. Login to GitHub using GitHub CLI, and follow the prompts to complete
   authorization.

        gh auth login

   Select "HTTPS" for preferred protocol.  
   Select "Y" to authenticate Git with GitHub credentials.  
   Select "Login with a web browser" to authenticate GitHub CLI.  
   If you see an error to open a web browser, Ctrl-left-click the link to open
   browser in Windows and paste the one-time code.

   Verify with

        gh auth status --show-token

## Generate signing keys with GPG

1. Verify gpg is installed.

        gpg --version

1. Generate ECDSA signing keys, and follow the sub-steps below.

        gpg --full-generate-key --expert

   1. Select **(9) ECC and ECC**.  
   1. Select **(3) NIST P-256**.  
   1. Select **default (0) = key does not expire**.  
   1. Enter your name and email as per your earlier git config. Leave blank for
      Comment.

1. Configure git to use signing key.

        git config --global commit.gpgsign true
        git config --global tag.gpgsign true
        gpg --list-secret-keys --keyid-format=long

    Copy the KEY_ID character sequence in "sec nistp256/KEY_ID", and use in next
    command.

        git config --global user.signingkey KEY_ID

    Verify the signingkey and gpgsign entries are registered

        git config --global --list

1. Configure gpg to direct prompts to user, and reload bashrc.

        [ -f ~/.bashrc ] && echo -e '\nexport GPG_TTY=$(tty)' >> ~/.bashrc
        source ~/.bashrc

   Verify gpg is working properly with

        echo "test" | gpg --clearsign

## Add GPG key to GitHub

1. Export GPG public key as ASCII text blob.

        gpg --armor --export KEY_ID

1. Copy GPG public key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----`
   and ending with `-----END PGP PUBLIC KEY BLOCK-----`.

1. Go to GitHub website -> Profile Picture -> Settings -> SSH and GPG Keys.
   Click "New GPG key" and paste the copied GPG public key.

## Clone GitHub repository to local Git

1. Go to GitHub website -> REPOSITORY_NAME -> Code button -> Local
   -> Clone (HTTPS). Copy the WEB_URL for your repository.

1. In your Ubuntu terminal, clone the remote GitHub repo to local Git repo.

        git clone WEB_URL

   Verify remote repo is successfully cloned into local Git repo with

        cd REPOSITORY_NAME
        git status

## Configure VS Code to perform signed git commits

1. Install PIN entry module for GPG with the sub-steps below.

   1. [Download](https://gpg4win.org/download.html) and install Gpg4win in
      Administrator mode.
   1. Install only GnuPG and Kleopatra components.

1. Inside WSL2, configure GPG to use the GUI PIN entry program with the
   sub-steps below.

   1. Create `~/.gnupg/gpg-agent.conf` with the following contents.

          default-cache-ttl 34560000  
          max-cache-ttl 34560000  
          pinentry-program "/mnt/c/Program Files (x86)/GnuPG/bin/pinentry-basic.exe"

   1. Restart gpg agent.

          gpgconf --kill gpg-agent

1. Verify the GUI PIN entry configuration with command below. A Windows PIN
   entry GUI should pop up.

        echo "test" | gpg --clearsign

1. Ensure VS Code is connected to the local Git repository with sub-steps below.

   1. Click bottom-left icon "Open a Remote Window" -> "Connect to WSL".
      - Bottom-left icon will change to "WSL:Ubuntu 22.04".
   1. Click top-left Explorer icon -> "Open Folder" -> Choose your Git
      repository path in Ubuntu filesystem.
      - Explorer window will update and show Git repository files and folders.

1. Perform Git commands directly from VS Code GUI.

   1. Make your necessary changes to files and folders on your local Git.
   1. Click top-left Source Control icon.
      - Source Control window will show Staged Changes of your local Git
        repository.
   1. Click Commit button. Alternatively, you can click Commit pull-down icon
      -> "Commit and Push".
      - Type your Commit message and click the Tick icon.
      - Git push will succeed and the Source Control window will update to show
        no remaining Staged Changes.

## References

1. GitHub: Install Git on Debian/Ubuntu
   [[link](https://github.com/git-guides/install-git#debianubuntu)]

1. GitHub Docs: Setting your username in Git
   [[link](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git)]

1. GitHub Docs: Setting your commit email address
   [[link](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address)]

1. GitHub Docs: Caching your GitHub credentials in Git
   [[link](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git)]

1. GitHub Docs: Signing Commits
   [[link](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)]

1. GitHub Docs: Generating a new GPG key
   [[link](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key?platform=linux)]

1. GitHub Docs: Telling Git about your signing key
   [[link](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key?platform=linux)]

1. GitHub Docs: Adding a GPG key to your GitHub account [[link](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account#adding-a-gpg-key)]

1. Microsoft Ignite: Get started using Visual Studio Code with Windows Subsystem for Linux
   [[link](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode)]

1. VS Code Docs: Using Git source control in VS Code
   [[link](https://code.visualstudio.com/docs/sourcecontrol/overview#_git-support)]

1. How to sign your commits to GitHub using Visual Studio Code on Windows 10 and
   WSL2 [[link](https://www.39digits.com/signed-git-commits-on-wsl2-using-visual-studio-code)]
