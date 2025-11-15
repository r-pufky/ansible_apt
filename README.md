# Apt
APT common operations.

Manage APT packages, repositories, upgrades, and unattended upgrades with a
single call.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_apt/blob/main/meta/main.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_apt/blob/main/defaults/main)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.deb](https://github.com/r-pufky/ansible_collection_deb) collection.

## Example Playbook
Apply before base OS configuration or separately as a dependency for another
role. Use group vars to apply defaults to all managed systems.

Add sources, dist-upgrade, install packages, and enable automatic updates:
``` yaml
- name: 'Manage APT'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.apt'
  vars:
    apt_dist_upgrade_enable: true
    apt_unattended_enable: true
```

All aspects of APT may be managed with this role:

group_vars/all/vars/apt.yml
``` yaml
apt_apt_sources:
  - name: 'debian'
    types:
      - 'deb'
      - 'deb-src'
    uris: 'http://deb.debian.org/debian'
    suites:
      - 'trixie'
      - 'trixie-updates'
      - 'trixie-backports'
    components:
      - 'main'
      - 'contrib'
      - 'non-free'
      - 'non-free-firmware'
    signed_by: '/usr/share/keyrings/debian-archive-keyring.gpg'
  - name: 'debian-security'
    types:
      - 'deb'
      - 'deb-src'
    uris: 'http://deb.debian.org/debian-security'
    suites:
      - 'trixie-security'
    components:
      - 'main'
      - 'contrib'
      - 'non-free'
      - 'non-free-firmware'
    signed_by: '/usr/share/keyrings/debian-archive-keyring.gpg'
apt_dist_upgrade_enable: true
apt_pins:
  - name: 'nodejs_keep'
    package: 'nodejs'
    pin: 'origin deb.nodesource.com'
    pin_priority: 1001
    state: 'present'
  - name: 'yarn'
    state: 'absent'
apt_packages:
  - 'vim'
  - 'git'
  - 'git-lfs'
  - 'curl'
  - 'htop'
  - 'tmux'
apt_packages_absent:
  - 'mdadm'
apt_unattended_enable: true
apt_unattended_upgrades: 1
```

Apply the base role
``` yaml
- name: 'Apply base configuration'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.apt'
```

Install packages as part of another role
``` yaml
- name: 'Install service packages'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.apt'
  vars:
    apt_update_cache: true
    apt_packages: 'zfsutils-linux'
```
Both of these will automatically manage pins, sources, updates, and packages.

## Development
Configure [environment](https://r-pufky.github.io/ansible_collection_docs/ansible/environment)

Run all unit tests:
``` bash
molecule test --all
```

### Releases
Major release versions track Debian release versions:

* **[13.x.x](https://github.com/r-pufky/ansible_apt)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_apt/tree/12.x)**: 12 Bookworm.

### Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_apt/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
