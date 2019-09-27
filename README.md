[![Build Status](https://travis-ci.org/kso512/install-check_mk-server.svg?branch=master)](https://travis-ci.org/kso512/install-check_mk-server)

# [install-check_mk-server](https://galaxy.ansible.com/kso512/install-check_mk-server/)

An [Ansible](https://www.ansible.com/) [Role](http://docs.ansible.com/ansible/playbooks_roles.html#roles) to install [Check_MK RAW](http://mathias-kettner.com/check_mk_introduction.html) and set up an initial site.

All tasks are tagged with `install-check-mk-server`.

I do **NOT** recommend the default configuration for unprotected connection directly to the Internet, as the server configuration includes unencrypted HTTP access.

Tested with [Travis continuous integration](https://travis-ci.org/) on the following distributions:

- [CentOS-7](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7)
- [CentOS-6](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS6.9)
- [Debian 9 "Stretch"](https://www.debian.org/releases/stretch/)
- [Debian 10 "Buster"](https://www.debian.org/releases/buster/)
- [Ubuntu 14.04 LTS "Trusty Tahr"](http://releases.ubuntu.com/trusty/)
- [Ubuntu 16.04 LTS "Xenial Xerus"](http://releases.ubuntu.com/xenial/)
- [Ubuntu 18.04 LTS "Bionic Beaver"](http://releases.ubuntu.com/bionic/)

## Requirements

Required on host that executes role with APT:
- python-apt (python 2)
- python3-apt (python 3)

Required on host that executes role with YUM:
- EPEL
- yum
- perl-Net-SNMP (minimal CentOS7)

If the server has a firewall enabled, it may need to be altered to allow incoming packets on TCP port 80 for the web portal access, and/or TCP port 514, plus UDP ports 162 & 514 for event console input.

As with any modern Linux deployment, SELinux may come into play.

To fulfill these requirements, I recommend using another Ansible Role.  For example, this role from Jeff Geerling may be used to handle EPEL if needed:
https://galaxy.ansible.com/geerlingguy/repo-epel

## Role Variables

To enable multi-distro support, the role defines distro-specific variables with the [`include_vars` and `with_first_found`](http://docs.ansible.com/ansible/include_vars_module.html) mechanisms.

### Defaults

| Variable | Description | Value |
| -------- | ----------- | ----- |
| install_check_mk_server_adminpw | Optional password for `cmkadmin` user | undefined |
| install_check_mk_server_build | Build number included in RPM source filename | `38` |
| install_check_mk_server_prereqs | List of packages to install before installing Check_MK RAW | `apt-utils` `cron` `python-passlib` |
| install_check_mk_server_site | Name of initial Check_MK RAW 'site' to provision | `test` |
| install_check_mk_server_source | Filename of the installation source | `check-mk-raw-{{ install_check_mk_server_version }}_0.{{ ansible_distribution_release }}_amd64.deb`
| install_check_mk_server_source_url | URL of Check_MK RAW installation file to download | `https://mathias-kettner.de/support/{{ install_check_mk_server_version }}/{{ install_check_mk_server_source }}` |
| install_check_mk_server_version | Version of Check_MK RAW to install | `1.6.0p2` |
| install_check_mk_server_web_service | Name of the Apache2 service to control | `apache2` |

### CentOS Distro Overrides

| Variable | Description | Value |
| -------- | ----------- | ----- |
| install_check_mk_server_prereqs | List of packages to install before installing Check_MK RAW | `cronie` `python-passlib` |
| install_check_mk_server_source | Filename of the installation source | `check-mk-raw-{{ install_check_mk_server_version }}-el{{ ansible_distribution_major_version }}-{{ install_check_mk_server_build }}.x86_64.rpm`
| install_check_mk_server_web_service | Name of the Apache2 service to control | `httpd` |

### Ubuntu 18.04 Distro Overrides

| Variable | Description | Value |
| -------- | ----------- | ----- |
| install_check_mk_server_prereqs | List of packages to install before installing Check_MK RAW | `apache2` `apt-utils` `aptitude` `cron` `iproute2` `libfl2` `man` `python-passlib` `rsync` `xz-utils` |

## Dependencies

This role depends on no other roles.

## Example Playbook

Complete example:

    - hosts: monitoring-servers
      roles:
         - { role: install-check_mk-server, install_check_mk_server_site: boom }

## License

BSD

## Author Information

kso512 (Chris Lindbergh) with contributions from Github users:
- sylekta
- timorunge
- judouk
