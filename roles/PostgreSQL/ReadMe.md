Install a PostgreSQL server as per the Arch way 
(*I definitively prefer the Gentoo one, but it would
require deep changes in the provided systemd service and lead to problems*).

### Configuration

- **postgresql_listen_addresses** = which ip to listen to (`localhost` (default), `*` all, what you want)
- **postgresql_port** = Postgresql server's port
> [!WARNING]
> Ansible is not smart enought to handle port change.
> To change the port :
> - disable the port, run the playbook
> - apply the new setting

- **postgresql_allowed_network** = network allowed to connect. If not set, no remote connection allowed.

> [!CAUTION]
> It's strongly advised to review **pg_hba.conf** as the provided configuration is not secure at all.
