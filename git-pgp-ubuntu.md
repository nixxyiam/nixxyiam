# Setup Git and PGP on WSL2 Ubuntu for GitHub Repository

## Install git on WSL2 Ubuntu

1. Open Terminal -> Ubuntu 22.04.2 LTS.
2. Install latest version of git. [^1]

        sudo apt-get update
        sudo apt-get install git-all
   Verify with `git version`.
       
3. Set your git username. [^2]

        git config --global user.name "YOUR NAME"
   Verify with `git config --global user.name`.

4. Set your git commit email address. [^3]

        git config --global user.email "YOUR_EMAIL"
   Verify with `git config --global user.email`.




[^1]: GitHub: Install Git on Debian/Ubuntu [[link](https://github.com/git-guides/install-git#debianubuntu)]
[^2] GitHub: Setting your username in Git [[link](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git)]
[^3]: GitHub: Setting your commit email address [[link](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address#setting-your-commit-email-address-in-git)]
