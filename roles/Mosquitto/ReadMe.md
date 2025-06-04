Install a Mosquitto server

### Configuration

- **allow_anonymous** = Tell if anonymous login is allowed.
(value doesn't matter : as long as this variable is present, it's allowed)
- **listener** = Enable remote connection with the given port.
> [!WARNING]
> Ansible is not smart enough to handle port change.
> To change the port :
> - disable the port, run the playbook
> - apply the new setting

> [!CAUTION]
> It's strongly advised to review **mosquitto.conf** as the provided configuration is not secure at all.
