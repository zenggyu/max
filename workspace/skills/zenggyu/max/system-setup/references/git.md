# Git

This is the reference file for Git setup.

# Installation/Upgrade

Home page: <https://git-scm.com/>

Use `apt` to install/upgrade Git on Debian-based systems.

# Configuration

Basic configuration:

```bash
git config --global core.quotepath false
git config --global core.editor nano
git config --global core.filemode false
git config --global core.autocrlf true # Windows only
git config --global core.autocrlf input # Linux/macOS only
```

Git Credential Manager (GCM) comes with Git for Windows and macOS by default. For Linux, set up GCM with the following commands:

```bash
sudo apt install pass gpg
gpg --list-secret-keys # list and choose existing GPG keys, or
gpg --gen-key # create a new one if none exists
pass init <gpg-uid> # initialise password store
git-credential-manager configure
git config --global credential.credentialStore gpg
```

After the above setups, ask user for git username and email, and then configure them with the following commands:

```bash
git config --global user.name <user-name>
git config --global user.email <user-email>
```
