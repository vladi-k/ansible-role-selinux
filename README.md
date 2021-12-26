ansible-role-selinux
====

Manage SELinux.

Requirements
------------

* RHEL8+

Role Variables
--------------

* `selinux_packages` - list of SELinux packages to install.
* `selinux_mode` - SELinux mode.
* `selinux_policy` - SELinux policy.
* `selinux_booleans` - list of SELinux booleans, example:

```yaml
selinux_booleans:
  - name: httpd_use_nfs
    state: true
    persistent: true
```

* `selinux_fcontexts` - list of SELinux file context mapping definitions, example:

```yaml
selinux_fcontexts:
  - target: "/mnt/share(/.*)?"
    target_root: /mnt/share
    setype: samba_share_t
```

* `selinux_fcontext_relabel` - boolean to run `restorecon` when applying `selinux_fcontexts`.
* `selinux_custom_policies` - list of custon SELinux policies, example:

```yaml
selinux_custom_policies:
   - name: my_policy
     dest: /root
     policy: |
        module my_policy 1.0;

        require {
            type syslogd_t;
            class dir { getattr search read open };
        }

        allow syslogd_t:dir { search getattr read open };
```

Dependencies
------------

Collections:

* `ansible.builtin`
* `ansible.posix`

Example Playbook
----------------

```
- hosts: my_servers
  vars:
    selinux_booleans:
      - name: httpd_use_nfs
        state: true
        persistent: true
  roles:
    - ansible-role-selinux
```

License
-------

GPLv3

Author Information
------------------

Vladimir Vasilev (@vladi-k)
