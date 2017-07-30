[![Build Status](https://travis-ci.org/kso512/ansible-install-check_mk-server.svg?branch=master)](https://travis-ci.org/kso512/ansible-install-check_mk-server)

# [ansible-install-check_mk-server](https://galaxy.ansible.com/kso512/install-check_mk-server/)

An [Ansible](https://www.ansible.com/) [Role](http://docs.ansible.com/ansible/playbooks_roles.html#roles) to install [Check_MK RAW](http://mathias-kettner.com/check_mk_introduction.html) and set up an initial site.

I do **NOT** recommend the default configuration for unprotected connection directly to the Internet, as the server configuration includes unencrypted HTTP access.

Tested with [Travis continuous integration](https://travis-ci.org/) on the following distributions:

- [Debian 9 "Stretch"](https://www.debian.org/releases/stretch/)
- [Ubuntu 14.04 LTS "Xenial Xerus"](http://releases.ubuntu.com/xenial/)
- [Ubuntu 16.04 LTS "Trusty Tahr"](http://releases.ubuntu.com/trusty/)

## Requirements

If the server has a firewall enabled, it may need to be altered to allow incoming packets on TCP port 80 for the web portal access, and TCP port 514, and UDP ports 162 & 514 for event console input.

## Role Variables

| Variable | Description | Default Value |
| -------- | ----------- | ------------- |
| ansible_install_check_mk_server_version | Version of Check_MK RAW to install | `1.4.0p9` |
| ansible_install_check_mk_server_source_url | URL of Check_MK RAW installation file to download | `https://mathias-kettner.de/support/{{ ansible_install_check_mk_server_version }}/check-mk-raw-{{ ansible_install_check_mk_server_version }}_0.{{ ansible_distribution_release }}_amd64.deb` |
| ansible_install_check_mk_server_site | Name of initial Check_MK RAW 'site' to provision | `test` |

## Dependencies

This role depends on no other roles.

## Example Playbook

Complete example:

    - hosts: servers
      roles:
         - { role: ansible-install-check_mk-server, ansible_install_check_mk_server_site: boom }

## License

BSD

## Author Information

> Chris Lindbergh
