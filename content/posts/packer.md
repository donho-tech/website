+++
date = 2021-01-02
title = "Use packer to create a VM image on Hetzner cloud"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to use packer to create a VM image to use it as a base template on the Hetzner cloud.

What do you need to complete this tutorial?

* packer
* Hetzner cloud account and API token

What is packer?

Hashicorp packer is a tool to automatically create VM images. You can start from a base image, configure it (e.g. installing software or hardening) and store it in the Hetzner cloud for further usage. By using template scripts to automatically create images, it provides a reproducible and versionable way of creating base images for your cloud infrastructure.

A simple packer template

Here is a very simple template script starts from a Ubuntu 18.04 base image and uses a cloud-init script to install some packages and create a user.

Create a file `cloud_config.yaml`

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

Create a packer template named `base_template.json`

```
{
    "variables": {
        "hcloud_token": "{{env `HCLOUD_TOKEN`}}"
    },
    "sensitive-variables": &#91;"hcloud_token"],
    "builders": &#91;{
        "type": "hcloud",
        "token": "{{user `hcloud_token`}}",
        "image": "ubuntu-18.04",
        "location": "nbg1",
        "server_type": "cx11",
        "ssh_username": "root",
        "user_data_file": "cloud_config.yaml",
        "snapshot_name": "basetemplate_ubuntu_18_04",
        "snapshot_labels": { "name": "packer_ubuntu_18_04_docker" }
    }]
}
```

Setting your API token:

```
export HCLOUD_TOKEN="your_Hetzner_cloud_token"
```

Run packer:

```
packer build base_template.json
```

Log in to your Hetzner account and look for your newly created image.

Learn more
* Official packer documentation: <https://www.packer.io/intro>
* Official cloud-init docs: <https://cloudinit.readthedocs.io/en/latest/index.html>