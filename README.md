ansible-role-syncthing
======================
Install [Syncthing](https://syncthing.net/).

Requirements
------------
A Debian-based distribution.

Role Variables
--------------
All variables are optional.

```yaml
# If the repository/key changes or you want to host your own, change these:
syncthing_apt_key_id:
syncthing_apt_key_url:
syncthing_apt_repository:

# If you wish to change the syncthing user name or home:
syncthing_user:
syncthing_user_home:

# If you don't want to role to create and manage the user:
syncthing_manage_user: false
# You will need to create the user specified in syncthing_user manually
# _before_ using the role.
```

Dependencies
------------
None.

Example Playbook
----------------
```yaml
- hosts: servers
  roles:
    - { role: L-P.syncthing, become: true }
```

License
-------
MIT
