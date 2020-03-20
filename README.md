# Ansible role AD membership

Manage membership of managed host with Windows AD.

[![Build Status](https://travis-ci.org/Caseraw/ansible_role_ad_membership.svg?branch=master)](https://travis-ci.org/Caseraw/ansible_role_ad_membership) [<img src="https://img.shields.io/ansible/role/47179">](https://galaxy.ansible.com/caseraw/ansible_role_ad_membership) [<img src="https://img.shields.io/ansible/role/d/47179">](https://galaxy.ansible.com/caseraw/ansible_role_ad_membership) [<img src="https://img.shields.io/ansible/quality/47179">](https://galaxy.ansible.com/caseraw/ansible_role_ad_membership)

- [Ansible role AD membership](#ansible-role-ad-membership)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)
  - [Testing with Molecule](#testing-with-molecule)
  - [CI/CD with Travis CI](#cicd-with-travis-ci)
  - [Useful links](#useful-links)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure a package manager is available and configured with the correct package sources and repositories.
- Ensure privileged permissions are set for the user executing this role to:
  - Install and uninstall.
  - Edit files such as, `/etc/krb5.conf` and `/etc/sssd/sssd.conf`.
  - Manage `systemd` services for `realmd`, `sssd` and `addjobd`.
- Ensure network traffic to the Windows Domain Controller.

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| role_ad_membership_sa_username | Service account username for the AD (encrypted). |
| role_ad_membership_sa_password | Service account password for the AD (encrypted). |
| role_ad_membership_required_packages | List of required packages to install. |
| role_ad_membership_use_global_domain_controller | Whether to use the global AD controller based on domain. |
| role_ad_membership_ad_controller_random_selection | Whther to randomly select an AD controller or just the first in the list. |
| role_ad_membership_ad_controllers | A list of AD controllers. |
| role_ad_membership_computer_ou | The AD Organizational Unit to place the Computer object in. |
| role_ad_membership_netbios_max_length | Maximum character length for Netbios hostname check. |
| role_ad_membership_leave_ad | Whether to leave the AD and remove the Computer object from the OU. |
| role_ad_membership_allowed_group_list | Combined list of other lists that start with the name `role_ad_membership_allowed_group_list_`. |
| role_ad_membership_allowed_group_list_default | A default list of groups to allow. |
| role_ad_membership_molecule_dummy | Dummy switch to bypass entire converge playbook. |

## Example Playbook

```yaml
---
- name: Manage membership of managed host with Windows AD
  become: True
  gather_facts: True
  vars_files:
    - /path/to/vault/file.yml
  tasks:
    - import_role:
        name: ansible_role_ad_membership
      vars:
        role_ad_membership_required_packages:
          - openldap-clients
          - krb5-workstation
          - krb5-libs
          - adcli
          - realmd
          - authconfig
          - samba-client
          - samba-common
          - samba-common-tools
          - sssd
          - sssd-ad
          - sssd-krb5
          - oddjob
          - oddjob-mkhomedir
        role_ad_membership_use_global_domain_controller: False
        role_ad_membership_ad_controller_random_selection: False
        role_ad_membership_ad_controllers:
          - ad1.example.com
          - ad2.example.com
        role_ad_membership_computer_ou: OU=Servers,DC=example,DC=com
        role_ad_membership_netbios_max_length: 15
        role_ad_membership_leave_ad: False
        role_ad_membership_allowed_group_list_default:
          - Special-Group-01
          - super_special_group_01
        role_ad_membership_allowed_group_list_something:
          - Special-Group-02
          - super_special_group_02
        role_ad_membership_allowed_group_list_something_else:
          - Special-Group-03
          - super_special_group_03

...
```

## Useful shell commands

Discover AD controller and domain specifics.

```shell
dig -t SRV _ldap._tcp.ad.example.com
dig -t SRV _ldap._tcp.dc._msdcs.ad.example.com
```

## Additional documentation resources

The following links provide more information about **sssd** and it's usage.

- <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/windows_integration_guide/sssd-integration-intro>

## Testing with Molecule

This role is locally tested with the use of [Molecule](https://molecule.readthedocs.io/en/latest/), the configuration is located at: [molecule/default](molecule/default).  
The Molecule tests are run (using the [docker driver](https://molecule.readthedocs.io/en/latest/configuration.html#docker)) on [Dockerhub images](https://hub.docker.com/u/caseraw) built for this purpose:

- [CentOS](https://hub.docker.com/r/caseraw/ansible-molecule-centos)
- [Fedora](https://hub.docker.com/r/caseraw/ansible-molecule-fedora)

Some specific configurations might require a full OS instead of a minimal container image. In these use-cases make use of [molecule driver for vagrant](https://molecule.readthedocs.io/en/latest/configuration.html#vagrant) with the [libvirt provider](https://molecule.readthedocs.io/en/latest/configuration.html#molecule-vagrant-module). The Molecule driver and platform configuration part could look something like this:

```yaml
driver:
  name: vagrant
  provider:
    name: libvirt
platforms:
  - name: ansible_role_ad_membership-ansible-molecule-centos-7
    box: centos/7
    imemory: 1024
    cpus: 1
```

## CI/CD with Travis CI

This role uses [Travis CI](https://travis-ci.org/) to run online tests with the use of [Molecule](https://molecule.readthedocs.io/en/latest/) and pushes notifications to import the role into [Ansible Galaxy](https://galaxy.ansible.com/) once the tests are successful. The Travis CI configuration is located at the root of the Ansible role [.travis.yml](.travis.yml)

## Useful links

- GitHub repository: <https://github.com/Caseraw/ansible_role_ad_membership>
- Travis CI build status: <https://travis-ci.org/Caseraw/ansible_role_ad_membership>
- Ansible Galaxy role: <https://galaxy.ansible.com/caseraw/ansible_role_ad_membership>