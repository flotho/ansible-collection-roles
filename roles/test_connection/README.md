# [test_connection](#test_connection)

Test connection possibilities to your system.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/robertdebock/ansible-role-test_connection/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-test_connection/actions)|[![gitlab](https://gitlab.com/robertdebock/ansible-role-test_connection/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-test_connection)|[![quality](https://img.shields.io/ansible/quality/45921)](https://galaxy.ansible.com/robertdebock/test_connection)|[![downloads](https://img.shields.io/ansible/role/d/45921)](https://galaxy.ansible.com/robertdebock/test_connection)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-test_connection.svg)](https://github.com/robertdebock/ansible-role-test_connection/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.

```yaml
---
- name: converge
  hosts: all
  gather_facts: no

  roles:
    - role: robertdebock.roles.test_connection
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
# defaults file for test_connection

# What host to save the results to.
test_connection_report_host: localhost

# What directory to save results to.
test_connection_report_directory: /tmp

# The timeout to wait for a connection.
test_connection_timeout: 5
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/robertdebock/ansible-role-test_connection/blob/master/requirements.txt).

## [Status of used roles](#status-of-requirements)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap)|[![Build Status GitHub](https://github.com/robertdebock/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/robertdebock/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-bootstrap)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/ansible-role-test_connection/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|alpine|all|
|amazon|Candidate|
|el|8|
|debian|all|
|fedora|all|
|opensuse|all|
|ubuntu|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-test_connection/issues)

## [License](#license)

Apache-2.0

## [Author Information](#author-information)

[robertdebock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
