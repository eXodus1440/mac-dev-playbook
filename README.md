<img src="https://raw.githubusercontent.com/eXodus1440/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Development Ansible Playbook

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have a few manual installation steps, but at least it's all documented here.

This project has been forked from Jeff Geerling's [mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook)

## Installation

  1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
    
      If there's issues running `ansible` below, follow the steps [outlined here](https://stackoverflow.com/questions/59887436/importerror-cannot-import-name-packagefinder):
      1. List available updates for 'Command Line Tools' with: `softwareupdate --list`
      2. Apply the update with: `softwareupdate -i "Command Line Tools for Xcode-13.0"`
    
  2. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html):

     1. Run the following command to add Python 3 to your $PATH: 
     
        `export PATH="$HOME/Library/Python/3.8/bin:/opt/homebrew/bin:$PATH"`
     2. Upgrade Pip: `sudo pip3 install --upgrade pip`
     3. Install Ansible: `pip3 install ansible`

  3. Clone or download this repository to your local drive.
  
     ```bash
     mkdir -p -- "~/Development/GitHub" && cd -P -- "~/Development/GitHub"
     git clone git@github.com:eXodus1440/mac-dev-playbook.git
     ```
  4. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
  5. Run `ansible-playbook main.yml --ask-become-pass` inside this directory. Enter your macOS account password when prompted for the 'BECOME' password.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    `ansible-playbook main.yml -K --tags "dotfiles,homebrew"`

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

```yaml
homebrew_installed_packages:
  - cowsay
  - git
  - go

mas_installed_apps:
  - { id: 443987910, name: "1Password" }
  - { id: 498486288, name: "Quick Resizer" }
  - { id: 557168941, name: "Tweetbot" }
  - { id: 497799835, name: "Xcode" }

composer_packages:
  - name: hirak/prestissimo
  - name: drush/drush
    version: '^8.1'

gem_packages:
  - name: bundler
    state: latest

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

### Running preconfigured set of overrides

Work, which is using the overrides from [config.work.yml](https://github.com/eXodus1440/mac-dev-playbook/blob/master/config.work.yml): 

`ansible-playbook main.yml -e env=work --ask-become-pass`



Personal, which is using the overrides from [config.personal.yml](https://github.com/eXodus1440/mac-dev-playbook/blob/master/config.personal.yml): 

`ansible-playbook main.yml -e env=personal --ask-become-pass`
