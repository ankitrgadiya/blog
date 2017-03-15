---
layout: post
title: "How to setup git server"
date: 2017-03-15
subtitle: "A tutorial to set up personal git server with web interface"
categories: [2017, how-to, tutorial, linux]
permalink: /setup-git-server/
---

## Git

[Git](https://git-scm.org) is the leading
[version control](https://en.wikipedia.org/wiki/Version_control) system,
currently used by most of the
[Open Source Softwares](https://en.wikipedia.org/wiki/Open-source_software)
including [Gnome](https://git.gnome.org) and [Linux Kernel](https://kernel.org)
to name a few. It basically helps developers maintain softwares with ease. It is
considerably fast and reliable then other
[version control](https://en.wikipedia.org/wiki/Version_control) softwares. Git
stores everything in
[repositories](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository).
To learn more about [Git](https://git-scm.org), check this awesome book by Scott
Chacon and Ben Straub, [Pro Git](https://git-scm.com/book/en/v2).

## Hosted Git repositories

A lot of componies promoting
[Open Source Softwares](https://en.wikipedia.org/wiki/Open-source_software),
provide hosted public git repositories for free. This repositories can be viewed
by anyone, using [Internet](https://en.wikipedia.org/wiki/Internet). Some of
these companies also provide limited/unlimited private repositories, access of
which is limited to authorized people only. Few of the most widely used of them
are:

* [Github](https://github.com)
* [Gitlab](https://gitlab.com)
* [Bitbucket](https://bitbucket.org)

## Why do you need git server?

Sure, there are plenty of options out there, but some people prefer to host
there own repositories for security and privacy purposes or for internal use
only. For them, setting up git server can result in faster access to repositores,
and better workflow.
Also, most Enterprise now-a-days prefer to setup there own git servers.

## Setting up git server

Note: I did this on my [Raspberry Pi 3](https://www.raspberrypi.org/) running on
[Raspbian](https://www.raspbian.org/).

Its better to add a separate user for Git.

```
$ adduser git
sudo adduser git
Adding user `git' ...
Adding new group `git' (1002) ...
Adding new user `git' (1002) with group `git' ...
Creating home directory `/home/git' ...
Copying files from `/etc/skel' ...
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
Changing the user information for git
Enter the new value, or press ENTER for the default
	Full Name []: git
	Room Number []:
	Work Phone []:
	Home Phone []:
	Other []:
Is the information correct? [Y/n] y
```

Then you need to install some packages for SSH server and Git web front-end.

```
$ sudo apt-get install openssh-server cgit apache2
```

Next you have to configure your SSH server disable login by password
authentication.

```
$ sudo vim /etc/ssh/sshd_config

# Edit this

# Authentication:
LoginGraceTime 120
#PermitRootLogin without-password
StrictModes yes

# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes

# To this

# Authentication:
LoginGraceTime 120
PermitRootLogin no
StrictModes yes

# Change to no to disable tunnelled clear text passwords
PasswordAuthentication no
```

Now, to reload the configuration, we need to restart SSH.

```bash
$ sudo service ssh restart
```
Next thing you need to do is to enable SSH to start during boot.

```bash
$ sudo update-rc.d -f ssh enable 2 3 4 5
```

Now, you need to add your client machine's public key to
```.ssh/authorized_keys``` file one per line.

Next thing you need to setup is web front-end. We are using cgit for it. So,
we need to enable cgi module in Apache.

```bash
$ sudo a2enmod cgi
$ sudo service apache2 restart
```

Now add your git repositories in git user's home directory or create a new one.

```bash
$ su git
Password:

$ git init test.git --bare
```

To show git repository on web, we need to add it in ```/etc/cgitrc``` file.

```
$ vim /etc/cgitrc
```

Following is the format to add a git repository in ```cgitrc```.

```
# test
repo.url=test
repo.path=/home/git/test.git
repo.owner=Ankit R Gadiya
```

Once added you'll be able to see it at http://\<ip address or domain>/cgit.

Done!

## Optional step

If this setup is going to be used by a lot of people, then you probably want to
make sure, users cannot use shell with SSH, they can just perform git functions.
To do this we will change the default Bash shell of git user with git-shell.

```
$ sudo chsh -s /usr/bin/git-shell git
```

Although administrators still need default shell and still need to be able to
login normally to add or remove git repositories. To do this you can use the
following:

```
$ sudo -u git -s
```

If you are going to do this over and over again, add alias in .bashrc instead.

```
$ echo "alias git-user='sudo -u git -s'" >> ~/.bashrc
```
