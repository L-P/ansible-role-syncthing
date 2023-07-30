ansible-role-syncthing
======================
Install [Syncthing](https://syncthing.net/).

Requirements
------------
A Debian-based distribution, root or _become_ on the remote host.
Look at the [Syncthing documentation](https://docs.syncthing.net/users/firewall.html)
to see what ports to open. The firewall configuration is not handled in this role.

Role Variables
--------------
All variables are optional.  
The only way to set Syncthing options is to edit its `config.xml` file, see the
[Example Playbook](#example-playbook) section for details.

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
# see the Example Playbook section.
syncthing_fetch_config:

# Where the fetched files will be written.
syncthing_fetch_dir:

# Configuration files content
syncthing_config_cert:
syncthing_config_key:
syncthing_config_https_cert:
syncthing_config_https_key:
syncthing_config_config:
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
    - { role: l-p.syncthing }
```

The Syncthing configuration is dynamic and edited at runtime using the GUI.  
You will need to get the configuration from the host and write it in the
`syncthing_config_*` variables if you want to keep it safe and ensure it is the
configuration you actually have on the server next time you run the role.

**Failing to fetch the configuration before running the role again means that
all new devices/folders/configuration will be erased from the remote host.**

If you don't set the `syncthing_config_*` variables nothing will be overwritten
but you will lose your configuration if your server sets itself on fire.

```yaml
# Get the generated configuration
- hosts: servers
  roles:
    - { role: l-p.syncthing, syncthing_fetch_config: true }
```

You can use `lookup` to read the files and set the variables:

```yaml
syncthing_config_cert: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/.config/syncthing/cert.pem')}}"
syncthing_config_key: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/.config/syncthing/key.pem')}}"
syncthing_config_https_cert: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/.config/syncthing/https-cert.pem')}}"
syncthing_config_https_key: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/.config/syncthing/https-key.pem')}}"
syncthing_config_config: "{{lookup('file', 'files/{{ inventory_hostname }}/home/syncthing/.config/syncthing/config.xml')}}"
```

I advise to use `ansible-vault` to encrypt the keys, `lookup` will decrypt them
on the fly.

License
-------
MIT
