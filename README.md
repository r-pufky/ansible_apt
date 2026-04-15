# Apt
APT common operations.

Manage APT packages, repositories, upgrades, and unattended upgrades with a
single call.

## [Requirements][i]
Requires [r_pufky.deb][g] galaxy-ng collection.

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [packages][k] - APT package management options.

* [pin][l] - APT pinning options.

* [sources][m] - APT sources options.

* [unattended_upgrades][n] - Unattended upgrade options.

## Usage
Apply before base OS configuration or separately as a dependency for another
role. Use **group_vars** to apply to all managed systems.

### Example Playbooks

``` yaml
- name: 'Add sources, dist-upgrade, install packages, and enable automatic updates.'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.apt'
  vars:
    apt_srv_dist_upgrade_always: true
    apt_uu_enable: true
```

``` yaml
- name: 'All aspects of APT may be managed with this role.'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.apt'
  vars:
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
    apt_srv_dist_upgrade_always: true
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
    apt_uu_enable: true
    apt_uu_upgrades: 1
```

#### Install packages as part of another role
``` yaml
- name: 'Setup a new debian APT source and install a package'
  ansible.builtin.include_role:
    name: 'r_pufky.deb.apt'
  vars:
    apt_update_cache: true
    apt_packages: 'plexmediaserver'
    apt_apt_sources:
      - name: 'plexmediaserver'
        types:
          - 'deb'
        uris: 'https://repo.plex.tv/deb'
        suites:
          - 'public'
        components:
          - 'main'
        signed_by: 'https://downloads.plex.tv/plex-keys/PlexSign.v2.key'
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

### [Releases][b]

  Release | Debian | Ansible | Notes
 ---------|--------|---------|-------
  4.x.x   | 13     | 2.20    | Ansible 2.20, semantic versioning.
  3.x.x   | 13     | 2.18    | Migrate to Debian Trixie.
  2.x.x   | 12     | 2.18    | Use standardized libraries.
  1.x.x   | 12     | 2.18    | Migration from private repository.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]

[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_apt/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_deb
[i]: https://github.com/r-pufky/ansible_apt/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_apt/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_apt/tree/main/defaults/main/packages.yml
[l]: https://github.com/r-pufky/ansible_apt/tree/main/defaults/main/pin.yml
[m]: https://github.com/r-pufky/ansible_apt/tree/main/defaults/main/sources.yml
[n]: https://github.com/r-pufky/ansible_apt/tree/main/defaults/main/unattended_upgrades.yml
