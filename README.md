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

# Set this to false if you don't want to role to create and manage the user.
# You will need to create the user specified in syncthing_user manually
# _before_ using the role.
syncthing_manage_user:

# Set this to true to get the Syncthing configuration from the remote host,
# see the _Example Playbook_ section.
syncthing_fetch_config:

# Where the fetched files will be written.
syncthing_fetch_dir:
```

Dependencies
------------
None.

Example Playbook
----------------
```yaml
# Install Syncthing
- hosts: servers
  roles:
    - { role: L-P.syncthing, become: true }
```

The Syncthing configuration is dynamic and edited at runtime using the GUI.  
You will need to get the configuration from the host and write it in the
`syncthing_config_*` variables if you want to keep it safe and ensure it is the
configuration you actually have on the server next time you run the role.

**Failing to fetch the configuration before running the role again means that
all new devices/folder/configuration will be erased from the remote host.**

If you never set the `syncthing_config_*` variables nothing will be
overwritten but you will lose your configuration if your server sets
itself on fire.

An [enhancement suggestion](https://github.com/syncthing/syncthing/issues/3598)
has been issued to mitigate the issue.

```yaml
# Get the generated configuration
- hosts: servers
  roles:
    - { role: L-P.syncthing, become: true, syncthing_fetch_config: true }
```

License
-------
MIT
