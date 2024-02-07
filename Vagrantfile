# -*- mode: ruby -*-
# vi: set ft=ruby :

# DEFAULT PROVIDER 

ENV['VAGRANT_DEFAULT_PROVIDER'] = "virtualbox"

# VM PARAMETERS

VAGRANT_BOX = "fedora/39-cloud-base"        # Vagrant box 
VAGRANT_BOX_VERSION = "39.20231031.1"       # Image version
VBOX_DISPLAY_NAME = "fedora-39"             # VM name that you will see on virtualbox
VBOX_HOSTNAME = "fedora.homelab.com"        # Hostname mapped to linux VM
VBOX_RAM = 2500                             # VM ram size
VBOX_CORE = 2                               # VM Core size
VBOX_DISK_SIZE = "60GB"                     # VM Disk size

Vagrant.configure("2") do |config|


    # Inline variables
  
    $fedora = <<-'FD'

      echo "
      ____   ___   ___ _____ ____ _____ ____      _    ____  
     | __ ) / _ \ / _ \_   _/ ___|_   _|  _ \    / \  |  _ \ 
     |  _ \| | | | | | || | \___ \ | | | |_) |  / _ \ | |_) |
     | |_) | |_| | |_| || |  ___) || | |  _ <  / ___ \|  __/ 
     |____/ \___/ \___/ |_| |____/ |_| |_| \_\/_/   \_\_|    
                                                             
      "

      # Additional user other than default vagrant user
      # Password will be vagrant both custom user and vagrant user
      # Change the user name if required.
      
      CUSTOM_USERNAME="user1"
      echo "[USER ADDITION] Adding user ${CUSTOM_USERNAME}" && \
      sudo useradd -m -p $(openssl passwd -1 vagrant) -s /bin/bash -G wheel ${CUSTOM_USERNAME} && \
      sudo echo "${CUSTOM_USERNAME} ALL=(ALL) NOPASSWD: ALL" > "/etc/sudoers.d/${CUSTOM_USERNAME}-nopasswd"
      
      # Setting Custom Prompt with colour enabled - vagrant & custom user #
      echo "[CUSTOM PROMPT] Adding custom prompt for vagrant & ${CUSTOM_USERNAME} users" && \
      sudo echo 'export "PS1=\n\[\033[01;32m\]\u@\h\[\033[00m\] \[\033[01;34m\]\W\[\033[00m\] $ "' >> "/home/${CUSTOM_USERNAME}/.bashrc"
      sudo echo 'export "PS1=\n\[\033[01;32m\]\u@\h\[\033[00m\] \[\033[01;34m\]\W\[\033[00m\] $ "' >> "/home/vagrant/.bashrc"

      # Setting fastest mirror and parallel downloads for dnf package manager for better performance
      echo "[DNF SETTINGS] DNF settings change" && \
      echo "max_parallel_downloads=10" >> "/etc/dnf/dnf.conf"
      echo "fastestmirror=true" >> "/etc/dnf/dnf.conf"

      sudo dnf update --refresh

      # Packages #
        # System Tools - curl, wget, net-tools, iputils, syststat, screenfetch, tree
        # Shell - Bash(Default), Fish(Additional)
        # Editor - Neovim
        # Container - Podman
        # Version Control - git
        # Python - python3-pip, python3-virtualenv bpython

      echo "[INSTALLATION] Install Packages" && \
      sudo dnf install -y vim curl wget htop net-tools iputils sysstat screenfetch rsync tree fish podman buildah skopeo python3-pip python3-virtualenv bpython git

      # Modify user.name and user.email accordingly and disable the comments before running Vagrant up

      # git config --global user.name ${CUSTOM_USERNAME}               
      # git config --global user.email "${CUSTOM_USERNAME}@github.com"
      # git config --global core.editor "nvim"
      # git config --global init.defaultBranch main

      # Install ansible #
      sudo dnf install -y ansible

      # Stop & Disable firewalld if enabled # 
      sudo systemctl stop firewalld
      sudo systemctl disable firewalld
    
      # Disable selinux. Reboot machine for changes to take effect
      sudo sed -i 's/enforcing/disabled/g' /etc/selinux/config
      sudo sestatus

      # post message #
      echo "-------------------------------------------"
      echo "REBOOT MACHINE FOR CHANGES TO TAKE EFFECT  "
      echo "vagrant reload                             "
      echo "-------------------------------------------"

    FD

    # Will not check for box updates during every startup.
    config.vm.box_check_update = false
  
    # Will not update the guest additions.
    config.vbguest.auto_update = false
  
    # Disable default share
    config.vm.synced_folder ".", "/vagrant", disabled: true
  
    config.vm.define "fedora" do |fedora|
      fedora.vm.box = VAGRANT_BOX
      fedora.vm.box_version = VAGRANT_BOX_VERSION
      fedora.vm.hostname = VBOX_HOSTNAME
      fedora.vm.disk :disk, size: VBOX_DISK_SIZE, primary: true
      fedora.vm.provider "virtualbox" do |rs|
        rs.memory = VBOX_RAM
        rs.cpus = VBOX_CORE
        rs.customize ["modifyvm", :id, "--groups", "/SERVER"]
        rs.name = VBOX_DISPLAY_NAME
      end
      fedora.vm.provision "shell", inline: $fedora
    end
end
