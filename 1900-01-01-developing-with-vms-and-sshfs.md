title: Developing with VMs and SSHFS
date: 1900/01/01
author: Brian J Brennan
tags: development sshfs vm
draft: true

* why do development in a VM instead of on my computer
* why use SSHFS instead of virtualbox's shared folder dealy
* setting up sshfs
  * I'm doing this on Ubuntu 10.10
  
  * Make a key if you don't have one:
    `ssh-keygen`
  
  * Copy your ID to the host machine:
    `ssh-copy-id user@hostmachine`
  
  * Do the same thing for root.
  * `sudo apt-get install sshfs` easy enough
  
  * Add user to the fuse group:
    `sudo gpasswd -a user fuse` 
  
  * Create a place where you want to mount your stuff:
    
    sudo mkdir /mnt/development
    sudo chown root:fuse /mnt/development
    sudo chmod g+w /mnt/development
  
  * Let users specify allow_other:
     In /etc/fuse.conf -> uncomment or add line `user_allow_other`
 
  * Setup your FSTAB!
    `shfs#user@hostmachine:path/to/files /mnt/development  fuse defaults,noauto,user,allow_other,ServerAliveInterval=60,cache=no`
  
  * (Explain options)
  
  * I setup my fstab with the `noauto` option because I don't have a static IP  for my host machine, I use zeroconf. At the point mounting happens in the boot sequence, zeroconf isn't active, so I wait until I login and do `mount /mnt/development` if I need to.
