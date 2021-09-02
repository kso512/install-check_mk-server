# [install-check_mk-server](https://galaxy.ansible.com/kso512/install-check_mk-server/)

_This role is deprecated in favor of [checkmk_server](https://github.com/kso512/checkmk_server) which is a rebuild using better practices and naming conventions.  No further updates will be made to this repository/role._

An [Ansible](https://www.ansible.com/) [Role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html) to install [Check_MK RAW](https://checkmk.com/product/raw-edition) and set up an initial site.

All tasks are tagged with `install-check-mk-server`.

**I do NOT recommend the default configuration for unprotected connection directly to the Internet, as the server configuration includes unencrypted HTTP access.**

Tested manually with the [Ansible Role Test Shim Script from Jeff Geerling](https://gist.github.com/geerlingguy/73ef1e5ee45d8694570f334be385e181) on the following distributions:

- [CentOS-7](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS7)
- [CentOS-8](https://wiki.centos.org/Manuals/ReleaseNotes/CentOS8.1905)
- [Debian 9 "Stretch"](https://www.debian.org/releases/stretch/)
- [Debian 10 "Buster"](https://www.debian.org/releases/buster/)
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

To fulfill these requirements, I recommend using another Ansible Role.  For example, [this role from Jeff Geerling may be used to handle EPEL](https://galaxy.ansible.com/geerlingguy/repo-epel) if needed.

## Role Variables

To enable multi-distro support, the role defines distro-specific variables with the [`include_vars` and `with_first_found`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/include_vars_module.html) mechanisms.

### Defaults

| Variable | Description | Value |
| -------- | ----------- | ----- |
| install_check_mk_server_adminpw | Optional password for `cmkadmin` user | undefined |
| install_check_mk_server_build | Build number included in RPM source filename | `38` |
| install_check_mk_server_key_url | URL of Check_MK GPG key file to download | `https://download.checkmk.com/checkmk/Check_MK-pubkey.gpg` |
| install_check_mk_server_prereqs | List of packages to install before installing Check_MK RAW | `apache2` `apt-utils` `cron` `dpkg-sig` `python-passlib` |
| install_check_mk_server_site | Name of initial Check_MK RAW 'site' to provision | `test` |
| install_check_mk_server_source | Filename of the installation source | `check-mk-raw-{{ install_check_mk_server_version }}_0.{{ ansible_distribution_release }}_amd64.deb` |
| install_check_mk_server_source_url | URL of Check_MK RAW installation file to download | `https://download.checkmk.com/checkmk/{{ install_check_mk_server_version }}/{{ install_check_mk_server_source }}` |
| install_check_mk_server_version | Version of Check_MK RAW to install | `2.0.0p9` |
| install_check_mk_server_web_service | Name of the Apache2 service to control | `apache2` |

### CentOS Distro Overrides

| Variable | Description | Value |
| -------- | ----------- | ----- |
| install_check_mk_server_prereqs | List of packages to install before installing Check_MK RAW | `cronie` `python-passlib` |
| install_check_mk_server_source | Filename of the installation source | `check-mk-raw-{{ install_check_mk_server_version }}-el{{ ansible_distribution_major_version }}-{{ install_check_mk_server_build }}.x86_64.rpm` |
| install_check_mk_server_web_service | Name of the Apache2 service to control | `httpd` |

#### CentOS 8 Distro Overrides

| Variable | Description | Value |
| -------- | ----------- | ----- |
| install_check_mk_server_prereqs | List of packages to install before installing Check_MK RAW | `cronie` `python3-passlib` `graphviz-gd` |
| install_check_mk_server_source | Filename of the installation source | `check-mk-raw-{{ install_check_mk_server_version }}-el{{ ansible_distribution_major_version }}-{{ install_check_mk_server_build }}.x86_64.rpm` |
| install_check_mk_server_web_service | Name of the Apache2 service to control | `httpd` |

### Ubuntu 18.04 Distro Overrides

| Variable | Description | Value |
| -------- | ----------- | ----- |
| install_check_mk_server_prereqs | List of packages to install before installing Check_MK RAW | `apache2` `apt-utils` `aptitude` `cron` `dpkg-sig` `iproute2` `libfl2` `man` `python3-passlib` `rsync` `xz-utils` |

## Dependencies

This role depends on none other.

## Example Playbook

Complete example:

    - hosts: monitoring-servers
      roles:
         - { role: install-check_mk-server, install_check_mk_server_site: boom }

## License

[GNU General Public License version 2](https://www.gnu.org/licenses/gpl-2.0.txt)

## Author Information

> Chris Lindbergh @kso512 with contributions from Github users:

- sylekta
- timorunge
- judouk
- JWhy
