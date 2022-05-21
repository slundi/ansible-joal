# Role Name

Ansible role to install JOAL, a torrent ratio faker, as a Linux service.

It installs the default linux JRE package (default-jre-headless).

## Role Variables

| Variable          | Default     | Description |
|-------------------|:-----------:|-------------|
| **joal_path**     | `/opt/joal` | Where the program binary should be installed |
| **joal_java_xss** | 64k         | NOT IMPLEMENTED Set the Java thread stack size (number+unit: 64k for 64 KB) |
| **joal_java_xms** | 32m         | NOT IMPLEMENTED Specify the initial Java heap size (number+unit: 32m for 32 MB) |
| **joal_java_xmx** | 256m         | NOT IMPLEMENTED Specify the maximum heap size (number+unit: 1g for 1GB) |

### Data owner

| Variable       | Default | Description |
|----------------|:-------:|-------------|
| **joal_owner** | root    | Linux user that is the owner of the data |
| **joal_group** | root    | Linux group of the data |

### JOAL seeding configuration

| Variable       | Default | Description |
|----------------|:-------:|-------------|
| **joal_min_upload_rate** | 16    | Minimum upload speed in kbits |
| **joal_max_upload_rate** | 4096  | Maximum upload speed in kbits |
| **joal_simultaneous_seed** | 4   | How many torrents should be seeding at the same time |
| **joal_client** | qbittorrent-4.4.2 | The name of the .client file to use in `joal-conf/clients/` (without the .client extension) |
| **joal_keep_torrent_with_0_leecher** | true |  should JOAL keep torrent with no leechers or seeders. If yes, torrent with no peers will be seed at 0kB/s. If false torrents will be deleted on 0 peers reached |

### JOAL web UI

You can enable the web UI and configure it with some variables:

| Variable       | Default | Description |
|----------------|:-------:|-------------|
| **joal_config_path** | `/opt/joal` | Path to the joal-conf folder (ie: /home/slundi/joal-conf). It is used when starting the app (`--joal-conf=PATH_TO_CONF` argument) |
| **joal_webui** | true    | To enable the web context (`--spring.main.web-environment=true` argument) |
| **joal_port**  | 7041    | The port to be used for both HTTP and WebSocket connection (`--server.port=YOUR_PORT` argument) |
| **joal_path_prefix** | `joal` | Use your own complicated path here (this will be your first layer of security to keep joal secret). This is security though obscurity, but it is required in our case. This must contains only alphanumeric characters (no slash, backslash, or any other non-alphanum char) (`--joal.ui.path.prefix="SECRET_OBFUSCATION_PATH"` argument) |
| **joal_secret_token** | `change-me` | Use your own secret token here (this is some kind of a password, choose a complicated one) (`--joal.ui.secret-token="SECRET_TOKEN"` argument) |

Once joal is started head to: http://_localhost_:_port_/_SECRET_OBFUSCATION_PATH_/ui/ (obviously, replace SECRET_OBFUSCATION_PATH) by the value you had chosen The **joal_path_prefix** might seems useless but it's actually crucial to set it as complex as possible to prevent peoples to know that joal is running on your server.

## Example Playbook

```yaml
- hosts: all
  gather_facts: true
  remote_user: root
  vars:
    joal_owner: me
    joal_group: me
    joal_client: "deluge-1.3.15"
    joal_config_path: /home/me/joal
  roles:
    - joal
```

## License

MIT

## Author Information

This is my first Ansible role, I tried to follow the best practices while making it.
