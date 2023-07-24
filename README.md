# Foreman Katello Kickstart
[![Open Source Love svg2](https://badges.frapsoft.com/os/v2/open-source.svg?v=103)](https://github.com/ellerbrock/open-source-badges/)

This repository contains a Kickstart file to create the necessary base system for a [Foreman](https://www.theforeman.org/) installation with the [Katello](https://theforeman.org/plugins/katello/) plugin.

This is tailored to be used when installing Foreman as a Virtual Machine on [libvirt](https://libvirt.org/).

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
The hostname is **foreman.management.internal**

### NTP Server
Set Timezone is `Europe/Berlin` and the NTP Server is set to `172.16.100.254`.
The system clock is set to _UTC_

## Credentials
Please change the pre-set passwords as soon as possible. These are only inteded for a first login and should be changed
immediately.

### Password
For the root user the pre-set password is `almalinux`

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
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPzC1siqNv0h4YvOrJT7EjxZ1jdLF03hFC/wDYlAO5fZ foreman.management.internal
```
