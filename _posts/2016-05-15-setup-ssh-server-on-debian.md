---
layout: post
title: How to setup SSH server on Debian
description: A tutorial to setup and configure SSH server on Debian machines.
---

This is a tutorial for setting up SSH server on Debian or distros based on Debain.

## Steps:
<br />

* Install the openssh-server package.
```bash
$ sudo apt-get install openssh-server
```

* Configure it to start at boot.
```bash
$ sudo update-rc.d -f ssh remove
$ sudo update-rc.d -f ssh defaults
$ sudo update-rc.d -f ssh enable 2 3 4 5
```

* For more security, you can change the default keys.
```bash
$ cd /etc/ssh
$ sudo mkdir keys_orig
$ sudo mv ssh_host_* keys_orig
```

* Now, generate new keys.
```bash
$ sudo dpkg-reconfigure openssh-server
```

* For more customisation, you can edit the sshd_config file.
```bash
$ gedit /etc/ssh/sshd_config
```

* After all the changes restart ssh service.
```bash
$ sudo service ssh restart
```
* Done!
