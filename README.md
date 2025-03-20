Ansible Role: nslcd
===================

An Ansible role that configures **nslcd**, i.e. authentication via LDAP.

**Note:** PAM is not yet done with this role or through dependencies!


Table of Contents
-----------------

<!-- toc -->

- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Top-Level Playbook](#top-level-playbook)
  * [Role Dependency](#role-dependency)
- [License](#license)
- [Author Information](#author-information)

<!-- tocstop -->

Requirements
------------

- Ansible 2.9

Role Variables
--------------

First, `ldap` needs to be defined in `nsswitch.conf`:

```yml
nsswitch_passwd: [files, ldap]
nsswitch_group: [files, ldap]
nsswitch_shadow: [files, ldap]
```

Then, for `nslcd.conf`:

```yml
nslcd_uris:
  - ldaps://ldap.example.org

nslcd_base: 'dc=example,dc=org'
nslcd_user_base: 'ou=people,dc=example,dc=org'
nslcd_group_base: 'ou=group,dc=example,dc=org'

nslcd_user_filter: >-
  (&(|(appRights=foo)(uid=alice)(uid=bob))(nsrole=*self*))

nslcd_min_uid: 2000
```

For more information, read `man 5 nslcd.conf`.

**Note:** Currently, not all parameters of `nslcd.conf` can be configured. If
you need something, feel free to contribute!

Dependencies
------------

```yml
---

# requirements.yml

roles:

  - name: idiv_biodiversity.nsswitch
    src: https://github.com/idiv-biodiversity/ansible-role-nsswitch
    version: vX.Y.Z

  - name: idiv_biodiversity.nslcd
    src: https://github.com/idiv-biodiversity/ansible-role-nslcd
    version: vX.Y.Z

...
```


Example Playbook
----------------

### Top-Level Playbook

Write a top-level playbook:

```yml
---

- name: server
  hosts: group

  roles:
    - role: idiv_biodiversity.nslcd
      tags:
        - nslcd

...
```

### Role Dependency

Define the role dependency in `meta/main.yml`:

```yml
---

dependencies:

  - role: idiv_biodiversity.nslcd
    tags:
      - nslcd

...
```

License
-------

MIT

Author Information
------------------

This role was created in 2023 by [Christian Krause][author] aka [wookietreiber
at GitHub][wookietreiber], HPC cluster systems administrator at the [German
Centre for Integrative Biodiversity Research (iDiv)][idiv].

[author]: https://www.idiv.de/staff/christian-krause/
[idiv]: https://www.idiv.de/
[wookietreiber]: https://github.com/wookietreiber
[idiv_biodiversity.nsswitch]: https://galaxy.ansible.com/idiv_biodiversity/nsswitch
