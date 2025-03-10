# Macbook Ansible Playbook
This playbook installs and configures most of the software I use on my Macbook for software development. This is a fork of Jeff Geerling's [Mac playbook](https://github.com/geerlingguy/mac-dev-playbook). He has paid apps from Apple store and I don't so I am removing those bits on this version of playbook.

## Installation

  1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
  2. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html):

     1. Run the following command to add Python 3 to your $PATH: `export PATH="$HOME/Library/Python/3.9/bin:/opt/homebrew/bin:$PATH"`
     1. Install Ansible: `pip install ansible`

  3. Clone or download this repository to your local drive.
  4. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
  5. Run `ansible-playbook main.yml --ask-become-pass` inside this directory. Enter your macOS account password when prompted for the 'BECOME' password.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Use with a remote Mac

You can use this playbook to manage other Macs as well. The playbook doesn't even need to be run from a Mac at all! If you want to manage a remote Mac, either another Mac on your network, or a hosted Mac like the ones from [MacStadium](https://www.macstadium.com), you just need to make sure you can connect to it with SSH:

  1. (On the Mac you want to connect to:) Go to System Preferences > Sharing.
  2. Enable 'Remote Login'.

You can also enable remote login on the command line:

>     sudo systemsetup -setremotelogin on

Then edit the `inventory` file in this repository and change the line that starts with `127.0.0.1` to:

```
[ip address or hostname of mac]  ansible_user=ssh
```

Then copy your SSH key to the remote Mac using 
```
ssh-copy-id user@remote.mac
```

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `extra-packages` and `osx`.

    ansible-playbook main.yml -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

```yaml
homebrew_installed_packages:
  - git
  - go

npm_packages:
  - name: webpack

pip_packages:
  - name: mkdocs

configure_dock: true
dockitems_remove:
  - Launchpad
  - TV
dockitems_persist:
  - name: "Sublime Text"
    path: "/Applications/Sublime Text.app/"
    pos: 5
```

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Applications installed with Homebrew Cask

  - [anythingllm](https://anythingllm.com/)
  - [brave-browser](https://brave.com/)
  - [chromedriver](https://sites.google.com/chromium.org/driver/)
  - [firefox](https://www.mozilla.org/en-US/firefox/new/)
  - [font-meslo-lg-nerd-font](https://github.com/ryanoasis/nerd-fonts)
  - [google-chrome](https://www.google.com/chrome/)
  - [intellij-idea-ce](https://www.jetbrains.com/idea/)
  - [iterm2](https://iterm2.com/)
  - [kitty](https://github.com/kovidgoyal/kitty)
  - [licecap](http://www.cockos.com/licecap/)
  - [nikitabobko/tap/aerospace](https://github.com/nikitabobko/AeroSpace)
  - [sequel-ace](https://github.com/Sequel-Ace/Sequel-Ace)
  - [slack](https://slack.com/)
  - [sublime-text](https://www.sublimetext.com/)
  - [tradingview](https://www.tradingview.com/desktop/)
  - [trilium-notes](https://github.com/zadam/trilium)
  - [visual-studio-code](https://code.visualstudio.com/)

## Packages installed with Homebrew

  - autoconf
  - bash-completion
  - doxygen
  - eza
  - gettext
  - gifsicle
  - git
  - gh
  - go
  - gpg
  - httpie
  - iperf
  - iterm2
  - libevent
  - neovim
  - nmap
  - node
  - nvm
  - ollama
  - pass
  - pngpaste
  - ssh-copy-id
  - readline
  - openssl
  - pv
  - thefuck
  - sqlite
  - wget
  - wrk
  - zsh-autosuggestions
  - zsh-history-substring-search

Jeff's [dotfiles](https://github.com/geerlingguy/dotfiles) are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

## Ansible for DevOps

Check out [Ansible for DevOps](https://www.ansiblefordevops.com/), which teaches you how to automate almost anything with Ansible.

## Author

This original project was created by [Jeff Geerling](https://www.jeffgeerling.com/) (originally inspired by [MWGriffin/ansible-playbooks](https://github.com/MWGriffin/ansible-playbooks)).

[badge-gh-actions]: https://github.com/geerlingguy/mac-dev-playbook/actions/workflows/ci.yml/badge.svg
[link-gh-actions]: https://github.com/geerlingguy/mac-dev-playbook/actions/workflows/ci.yml
