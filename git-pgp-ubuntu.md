# Easy Guide to Setup git with GitHub and WSL2 Ubuntu

This guide aims to minimise time for developers to set up new Windows development systems that require Git, GitHub, GPG, and Visual Studio Code (VS Code) as part of the development workflow.

The following assumptions have been made to avoid an overly lengthly writeup:

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

2. Install latest version of git.

        sudo apt-get install git-all
   Verify with

        git version

3. Set your git username and commit email address.

        git config --global user.name "John Doe"
        git config --global user.email "john@johndoe.com"

   Verify with

       git config --global --list

## Install GitHub CLI and connect to GitHub

1. Install GitHub CLI.

       sudo apt-get install gh

2. Login to GitHub using GitHub CLI, and follow the prompts to complete authorisation.

       gh auth login

   Select "HTTPS" for preferred protocol.  
   Select "Y" to authenticate Git with GitHub credentials.  
   Select "Login with a web browser" to authenticate GitHub CLI.  
   If you see an error to open a web browser, Ctrl-left-click the link to open browser in Windows and paste the one-time code.

   Verify with

       gh auth status --show-token

## Generate signing keys with GPG

1. Verify gpg is installed.

       gpg --version

2. Generate ECDSA signing keys.

       gpg --full-generate-key --expert

   Select (9) ECC and ECC.  
   Select (3) NIST P-256.  
   Select default (0) = key does not expire.  
   Enter your name and email as per your earlier git config. Leave blank for Comment.

3. Configure git to use signing key.

       git config --global commit.gpgsign true
       gpg --list-secret-keys --keyid-format=long

    Copy the KEY_ID character sequence in "sec nistp256/KEY_ID", and use in next command.

       git config --global user.signingkey KEY_ID

    Verify the signingkey and gpgsign entries are registered

       git config --global --list

4. Configure gpg to direct prompts to user, and reload bashrc.

       [ -f ~/.bashrc ] && echo -e '\nexport GPG_TTY=$(tty)' >> ~/.bashrc
       source ~/.bashrc

   Verify gpg is working properly with

       echo "test" | gpg --clearsign

## Add GPG key to GitHub

1. Export GPG public key as ASCII text blob.

       gpg --armor --export KEY_ID

2. Copy GPG public key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`.

3. Go to GitHub website -> Profile Picture -> Settings -> SSH and GPG Keys. Click "New GPG key" and paste the copied GPG public key.

## Clone GitHub repository to local Git

1. Go to GitHub website -> REPOSITORY_NAME -> Code button -> Local -> Clone (HTTPS). Copy the WEB_URL for your repository.

2. In your Ubuntu terminal, clone the remote GitHub repo to local Git repo.

        git clone WEB_URL

   Verify remote repo is successfully cloned into local Git repo with

       cd REPOSITORY_NAME
       git status

## Configure VS Code as Git Editor

To be filled in...

## References

1. GitHub: Install Git on Debian/Ubuntu [[link](https://github.com/git-guides/install-git#debianubuntu)]

2. GitHub Docs: Setting your username in Git [[link](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git)]

3. GitHub Docs: Setting your commit email address [[link](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address)]

4. GitHub Docs: Caching your GitHub credentials in Git [[link](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git)]

5. GitHub Docs: Signing Commits [[link](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)]

6. GitHub Docs: Generating a new GPG key [[link](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key?platform=linux)]

7. GitHub Docs: Telling Git about your signing key [[link](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key?platform=linux)]

8. GitHub Docs: Adding a GPG key to your GitHub account [[link](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account#adding-a-gpg-key)]
