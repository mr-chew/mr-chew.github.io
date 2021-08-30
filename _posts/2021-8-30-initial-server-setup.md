---
layout: post
title:  Initial Server Setup
categories: [Debian]
---

Getting our new vm running Debian 11 ready. 
After logging in as root (details from provider email), we want to create a new user account, root account will be disabled thereafter.


```bash
ssh root@my_server_ip
adduser debian12345
```

Granting our new user account with Adminstrative privileges, thereafter try logging with the new account to ensure you can log in
```bash
usermod -aG sudo debian12345
```
if faced with the below error message
```
sudo command not found.
```
sudo needs to be installed
```bash
apt install sudo
```
Next on the list is to change the ssh port, reduce login grace period and disable root login
```bash
sudo nano /etc/ssh/sshd_config
Port 54781
LoginGraceTime 1m
PermitRootLogin No
```
After saving the sshd_config file, restart the ssh daemon
```bash
sudo systemctl restart ssh
```
Lock the root account
```bash
sudo usermod -L root
```
