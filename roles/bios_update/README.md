# [bios_update](#bios_update)

Download, extract and write bootable USB image.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/robertdebock/ansible-role-bios_update/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bios_update/actions)|[![gitlab](https://gitlab.com/robertdebock/ansible-role-bios_update/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-bios_update)|[![quality](https://img.shields.io/ansible/quality/39155)](https://galaxy.ansible.com/robertdebock/bios_update)|[![downloads](https://img.shields.io/ansible/role/d/39155)](https://galaxy.ansible.com/robertdebock/bios_update)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-bios_update.svg)](https://github.com/robertdebock/ansible-role-bios_update/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.

```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.roles.bios_update
      # The bios_update_url does not always need to be set, it's typically
      # "discovered". For CI however, there is not right model, so this
      # variable needs to be set manually.
      bios_update_url: "https://download.lenovo.com/pccbbs/mobiles/r02uj70d.iso"
      # In CI, it's hard to write to a removable media, this parameter basically
      # disables writing.
      bios_update_write: no
```

The machine needs to be prepared. In CI this is done using `molecule/default/prepare.yml`:

```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.roles.bootstrap
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in `defaults/main.yml`:

```yaml
---
# defaults file for bios_update

# Where to store the downloaded ISO.
bios_update_temporary_directory: /tmp

# The URL to a bootable ISO containing a BIOS update.
# The URL is "discovered" in `vars/main.yml`, but can be overwritten here.
# or in any variable that takes precedence.
#
# bios_update_url: "https://download.lenovo.com/pccbbs/mobiles/r02uj70d.iso"

# The device to write the bootable image to.
#
# WARNING: THIS DEVICE WILL BE OVERWRITTEN.
#
bios_update_flash_drive: "/dev/sdCHANGEME"

# By default this role should write to removable media. Can be disabled in CI.
bios_update_write: yes
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/robertdebock/ansible-role-bios_update/blob/master/requirements.txt).

## [Status of used roles](#status-of-requirements)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap)|[![Build Status GitHub](https://github.com/robertdebock/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/robertdebock/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-bootstrap)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/ansible-role-bios_update/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|amazon|Candidate|
|el|8|
|debian|all|
|fedora|all|
|ubuntu|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-bios_update/issues)

## [License](#license)

Apache-2.0

## [Author Information](#author-information)

[robertdebock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
