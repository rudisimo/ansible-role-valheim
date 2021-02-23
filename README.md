# Ansible Role: Valheim

Installs Valheim Dedicated Server on Debian/Ubuntu servers.

This roles installs and configures the latest version of Valheim Dedicated Server from Steam.

## Requirements

This role depends on [SteamCMD](https://developer.valvesoftware.com/wiki/SteamCMD) to install and configure the Valheim Dedicated Server binaries. Use the following Ansible Role to install it: [rudisimo/ansible-role-steamcmd](https://github.com/rudisimo/ansible-role-steamcmd)

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

The directories where Valheim Dedicated Server binaries will be installed, and the server's save data will be stored.

```yaml
valheim_install_path: /usr/local/games/valheim-server
valheim_saves_path: /usr/local/share/valheim-server/data
```

If set to true, Valheim Dedicated Server files, managed by this role's variables (see `valheim_servers`), will be created. You can also override the templates used and set a path to your own templates, if you need to further customize the server configuration.

```yaml
valheim_add_servers: true
valheim_server_script_template: valheim-server.sh.j2
valheim_server_service_template: valheim-server.service.j2
```

Add a set of properties per server, including `name` (required), `password` (required), `port` (required), `world` (optional), and `savedir` (optional: defaults to the value of `valheim_saves_path`).

```yaml
valheim_servers:
  - name: Valheim Dedicated Server
    password: secretpassword
    port: 2456
```

Here's an example using a custom `savedir`:

```yaml
valheim_servers:
  - name: Valheim Dedicated Server
    password: secretpassword
    port: 2456
    world: Dedicated
    savedir: /mnt/valheim-server/hellheim
```

The user permissions are empty by default, but you can add them below to be applied globally to all servers.

```yaml
valheim_global_administrators: []
valheim_global_banned_users: []
valheim_global_permitted_users: []
```

## Example Playbook

```yaml
- hosts: all
  roles:
    - { role: rudisimo.valheim }
  vars:
    steam_install_path: /opt/steam
    steam_user: steam

    valheim_add_servers: true
    valheim_install_path: /usr/local/games/valheim-server
    valheim_saves_path: /usr/local/share/valheim-server/data
    valheim_servers:
      - name: Valheim
        port: 27001
        password: deadbeef
      - name: Miehlav
        port: 27100
        password: feebdaed
```

## License

MIT
