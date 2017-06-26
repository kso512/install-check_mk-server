[![Build Status](https://travis-ci.org/kso512/ansible-install-check_mk-server.svg?branch=master)](https://travis-ci.org/kso512/ansible-install-check_mk-server)

# ansible-install-check_mk-server

A role to install Check_MK RAW and set up an initial site.

## Requirements

Uses the following Core Ansible modules:

- apt
- command

## Role Variables

| Variable | Description | Default Value |
| -------- | ----------- | ------------- |
| ansible_install_check_mk_server_version | Version of Check_MK RAW to install | `1.4.0p6` |
| ansible_install_check_mk_server_source_url | URL of Check_MK RAW installation file to download | `https://mathias-kettner.de/support/{{ ansible_install_check_mk_server_version }}/check-mk-raw-{{ ansible_install_check_mk_server_version }}_0.{{ ansible_distribution_release }}_amd64.deb` |
| ansible_install_check_mk_server_site | Name of initial Check_MK RAW 'site' to provision | `test` |

## Dependencies

Depends on APT/Debian support.

## Example Playbook

Complete example:

    - hosts: servers
      roles:
         - { role: ansible-install-check_mk-server, ansible_install_check_mk_server_site: boom }

## License

BSD

## Author Information

> Chris Lindbergh
