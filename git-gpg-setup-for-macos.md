# Git GPG Setup for macOS

## Packages

### Install gnupg

```bash
brew install gpg
```

### Install passphrase entry dialogs

```bash
brew install pinentry-mac
```

## Generate a key

Follow instructions, select default via `Enter` if unsure.

```bash
gpg --full-generate-key
```

## Identify your key:

```bash
gpg --list-secret-keys --keyid-format=long
```

Your `<key>` is in "sec" part after slash, eg: sec ed25519/HERE 2021-12-07 [SC]

## NOTE: M1 macs or freshly installed brew

Check where brew is located itself:

```
which brew
```

Substitute all `/usr/local/bin` locations in the paths below to: `/opt/homebrew/bin`.

## Set git settings

```bash
git config --global user.signingkey <key>
git config --global commit.gpgsign true
git config --global gpg.program /usr/local/bin/gpg
```

## Additional config

```bash
if [ -r ~/.zshrc ]; then echo 'export GPG_TTY=$(tty)' >> ~/.zshrc; \
	else echo 'export GPG_TTY=$(tty)' >> ~/.zprofile; fi
```

```bash
echo "pinentry-program /usr/local/bin/pinentry-mac" > ~/.gnupg/gpg-agent.conf
```

## Restart gpg service

```bash
gpgconf --kill gpg-agent
```

## Add your key to GitHub

### Output your public key:

```bash
gpg --armor --export <key>
```

### Add it to GitHub here:

[https://github.com/settings/gpg/new](https://github.com/settings/gpg/new)

## Second Machine

If you are moving your gpg folder (`~/.gnupg`) to another machine, make sure to correct permissions afterwards:

```
chown -R $(whoami) ~/.gnupg/
chmod 600 ~/.gnupg/*
chmod 700 ~/.gnupg
```

## NOTE: M1 macs or freshly installed brew

Some tools may be hardcoded to look only in one location, make a symlink:

```
sudo ln -s /opt/homebrew/bin/gpg /usr/local/bin
```
