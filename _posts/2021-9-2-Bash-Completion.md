---
layout: post
title:  Bash Completion
categories: [Debian, Bash]
---

Bash Completion is a useful feature where we type a partial command and press Tab to complete it. However it is not enabled in Debian by default. 
First we need to install it and then update the profile file.

```bash
sudo apt install bash-completion
```

Edit /etc/profile and insert the following codes
```bash
if [ -f /etc/bash_completion ]; then
 . /etc/bash_completion
fi
```

Thereafter log out and log back in again and bash completion should be available.
