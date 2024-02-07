# FEDORA SERVER SETUP WITH VAGRANT & VIRTUALBOX

The repository contains Vagrantfile for setting up fedora in virtualbox. Fedora releases an official vagrant box for each release, which the Fedora team maintains. 

<https://app.vagrantup.com/fedora>

The vagrantfile is created with [Fedora 39 cloud base](https://app.vagrantup.com/fedora/boxes/39-cloud-base). In future, you can modify the box version alone using the same file.

## QUICK SETUP

1. Clone the repo to your local machine.

```sh
$ git clone https://github.com/Devrunops/fedora-vagrant
```

2. Run the vagrant up command to spin up the machine.

```sh
$ cd fedora-vagrant
$ vagrant up
```

3. Wait for the vagrant to set up the machine and reboot the vagrant machine once the setup is completed.

```sh
$ vagrant reload
```

4. Connect to the vagrant box. You should be within the folder where vagrantfile is present to run the command.

```sh
$ vagrant ssh
```

In VirtualBox, the machine will be grouped under `SERVER`.

![image](https://github.com/Devrunops/fedora-vagrant/assets/156952563/fa9b7941-c817-4012-bcb9-39c417d6bc81)

## SHELL PROVISIONER

The `shell provisioner` is used to bootstrap the machine. 

- A new user will be created other than the default vagrant user. Modify the `CUSTOM_USERNAME` variable to change the user name. The user will be allocated with passwordless sudo privilege similar to the vagrant user.
- The following packages will be installed.
    - System Tools - curl, wget, net-tools, iputils, syststat, screenfetch, tree, rsync
    - Shell - Bash(Default), Fish(Additional)
    - Editor - Neovim
    - Container - podman buildah skopeo
    - Version Control - git
    - Python - python3-pip, python3-virtualenv bpython
    - Config Management - Ansible
- Git config is commented out. Uncomment, and change it accordingly before running the `vagrant up` command.
- SELinux & Firewall is disabled if enabled. Reboot the machine for changes to take effect.
