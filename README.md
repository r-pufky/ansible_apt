# Apt
APT common operations.

Manage APT packages, repositories, upgrades, and unattended upgrades with a
single call.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_apt/blob/main/meta/main.yml)

[collections/roles](https://github.com/r-pufky/ansible_apt/blob/main/meta/requirements.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_apt/blob/main/defaults/main)

## Dependencies
Part of the [r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv)
collection.

## Example Playbook
Apply before base OS configuration or separately as a dependency for another
role. Use group vars to apply defaults to all managed systems.

group_vars/all/vars/apt.yml
```
apt_apt_sources:
  - name: 'debian'
    types:
      - 'deb'
      - 'deb-src'
    uris: 'http://deb.debian.org/debian'
    suites:
      - 'bookworm'
      - 'bookworm-updates'
      - 'bookworm-backports'
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
      - 'bookworm-security'
    components:
      - 'main'
      - 'contrib'
      - 'non-free'
      - 'non-free-firmware'
    signed_by: '/usr/share/keyrings/debian-archive-keyring.gpg'
apt_dist_upgrade_enable: true
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
    name: 'r_pufky.srv.apt'
```

Install packages as part of another role
``` yaml
- name: 'Install service packages'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.apt'
  vars:
    apt_update_cache: true
    apt_packages: 'zfsutils-linux'
```
Both of these will automatically manage sources, updates, and packages.

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_srv/blob/main/docs/dev/environment/README.md)

Run all unit tests:
``` bash
molecule test --all
```

### Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_apt/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
