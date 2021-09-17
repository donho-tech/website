+++
date = 2020-08-19T19:55:18+02:00
title = "Use cloud-init to prepare your cloud VMs"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

Learn what cloud-init is and how to use it.

To test your cloud-init script, you can use any of the big cloud providers (Azure, AWS, GCC...).

What is cloud-init?

Cloud-init is a tool to prepare your cloud Linux VMs before using. For example, you could install necessary software to
create a CI/CD pipeline like Docker, Nomad by installing the corresponding packages or harden your VM by configuring
intrusion detection software, disabling root login and setting a non-standard SSH port. You can also read metadata about
the cloud environment and configure your VM based on that.

It is available on all major Linux distributions and cloud platforms.

A simple cloud-init script

Here is a very simple and self-explanatory cloud-configuration which creates a user and installs some packages. You can
test it by creating a VM on the cloud platform of your choice which should give you an option to provide it for
initialization.

```
#cloud-config

groups:
- ninjaturtles
users:
- name: raphael
  groups: ninjaturtles
  lock_passwd: false
  # created with 'openssl passwd -1 -salt 12345 test'
  # password=test
  passwd: $1$12345$CQruoOSylf9ItA76Hhqh2.

package_upgrade: true
package_update: true
packages:
- git
- htop
- vim
```

Log in to the new VM with the configured user and password and browse the cloud-init logs, which can be found
under <code>/var/log/cloud-init.log