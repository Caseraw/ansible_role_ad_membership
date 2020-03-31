# CHANGELOG

## v0.2.1

Variable name issue in the `sssd.conf` template.

Fixed `role_ad_membership_ou_search_base` into `role_ad_membership_ou_user_search_base`

## v0.2.0

Fixed additional unforeseen issues.

**Functionalities:**

- Heavy prerequisite check.
- Heavy set of logic states.
- Joining a domain always adds an AD computer object prior to joining.
- Leaving a domain always removes the AD computer and removes sss cache.
- sssd.conf relies on GSSAPI and Kerberos.

## v0.1.0

Creation of the Ansible role AD membership.

**Functionalities:**

- Join an AD.
- Leave an AD.
- Managed groups to allow.
- Managed SSSD and Kerberos configuration files.
