# Foreman Katello Kickstart
[![Open Source Love](https://badges.frapsoft.com/os/v1/open-source.png?v=103)](https://github.com/ellerbrock/open-source-badges/)
[![GPL Licence](https://badges.frapsoft.com/os/gpl/gpl.png?v=103)](https://opensource.org/licenses/GPL-3.0/)

This repository contains a Kickstart file to create the necessary base system for a [Foreman](https://www.theforeman.org/) installation with the [Katello](https://theforeman.org/plugins/katello/) plugin.
Also, a Kickstart file the necessary base system for a [RedHat Satellite](https://www.redhat.com/technologies/management/satellite) installation is created.

With these kickstart files an ISO is created for each distribution and attached to the individual releases.
This ISO should be present as disc during the installation for a fully automated / unattended installation.
The corresponding ISO must be renamed to **OEMDRV.iso** when attached to the system.

This is tailored to be used when installing Foreman/Satellite as a Virtual Machine on [libvirt](https://libvirt.org/).

For a RedHat installation the DVD ISO is required, it will not work with the minimal / boot ISO.

## System / VM requirements
The VM should have at least _88GiB_ on the `vda` disk.

## Installed packages
Besides the `@Core` package group only the packages
- `kexec-tools`
- `langpacks-de`
- `langpacks-en`
- `python36`

are installed. The packages
- `microcode_ctl`
- `iwl*firmware`
- `plymouth`

are removed/blocked since they are not needed or do not work inside a VM.

After this base setup the further installation and configuration of Foreman with Katello is done with [Ansible](https://www.ansible.com/).

## Network Configuration 
The Network interface `enp1s0` is set to _static_ with the following parameters
```yaml
IP: 172.16.100.20
GATEWAY: 172.16.100.254
NETMASK: 255.255.255.0
NAMESERVER: 172.16.100.254
```
IPv6 is disabled on the interface.

### Hostname
The hostname is either **satellite.management.internal** or **foreman.management.internal**

### NTP Server
Set Timezone is `Europe/Berlin` and the NTP Server is set to `172.16.100.254`.
The system clock is set to _UTC_

## Credentials
Please change the pre-set passwords as soon as possible. These are only inteded for a first login and should be changed
immediately.

### Password
For the root user the pre-set password is either `almalinux` or `redhat`.

### SSH Key
Remote login for the root user is only allowed with an SSH-Key (prohibit-password) where the details are listed below

_Private Key_
```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACD8wtbIqjb9IeGLzqyU+xI8WdY3SxdN4RQv8A2JQDuX2QAAAJhd00UfXdNF
HwAAAAtzc2gtZWQyNTUxOQAAACD8wtbIqjb9IeGLzqyU+xI8WdY3SxdN4RQv8A2JQDuX2Q
AAAEBL/pYo4CGMibFrzV2LU5Ka2B2yRtx57uWjSymJ4dh8bvzC1siqNv0h4YvOrJT7EjxZ
1jdLF03hFC/wDYlAO5fZAAAAEmRlbm5pc0B3b3Jrc3RhdGlvbgECAw==
-----END OPENSSH PRIVATE KEY-----
```

_Public Key_ (already present in authorized_keys of user root)
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPzC1siqNv0h4YvOrJT7EjxZ1jdLF03hFC/wDYlAO5fZ _COMMENT_
```
where _\_COMMENT__ is either `satellite.management.internal` or `foreman.management.internal`.

## Links

Additional documentation and Links can 

- [Kickstart commands and options reference](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/performing_an_advanced_rhel_8_installation/kickstart-commands-and-options-reference_installing-rhel-as-an-experienced-user#addon-com_redhat_kdump_kickstart-commands-for-addons-supplied-with-the-rhel-installation-program)