# ROUTER-SETUP

An ansible role for setting up a machine to run as a router with its own dns and dhcp capabilities.

</br>

## Configuration

Configuration of variables is done in the inventory file.
The script expects the inventory to be at inventory/hosts.yaml. A different location can be specified using the `-i` flag.

The variables must be part of the hostvars for which the playbook is run.

### Main

| Key           | Required | Default | Description                             |
| ------------- | -------- | ------- | --------------------------------------- |
| wan_interface | yes      | -       | The interface to the "internet"         |
| zones         | yes      | -       | List of zones managed by the dns server |

### Main.Zones[]

| Key                       | Required | Default | Description                                              |
| ------------------------- | -------- | ------- | -------------------------------------------------------- |
| name                      | yes      | -       | Domain name of the zone                                  |
| lan_interface             | yes      | -       | The interface to the subnet the server is the router for |
| lan_ip                    | yes      | -       | First three octets for the subnet                        |
| allowed_ips               | no       | -       | List of whitelisted subnets allowed to query that zone   |
| dhcp_dynamic_range_bottom | yes      | 100     | Lowest value available for dynamic dhcp                  |
| dhcp_dynamic_range_top    | yes      | 199     | Highest value available for dynamic dhcp                 |
| dns                       | yes      | -       | List of static ips in the zone                           |

### Main.Zones[].dns[]

| Key      | Required | Default | Description                                 |
| -------- | -------- | ------- | ------------------------------------------- |
| hostname | yes      | -       | Hostname of the machine                     |
| ip       | yes      | -       | Static ip of the machine                    |
| MAC      | yes      | -       | MAC address of the interface of the machine |
| cnames   | no       | -       | List of CNAMES associated with the machine  |

</br>

## Run

```
ansible-playbook router.yaml
```

The following tags can be added by using `--tags <tag>`:

- install (Run full playbook)
- setup (Run system, network, and application configuration)
- iptables (Deploy iptable rules)

_Note that application configuration always runs_

## Notes

- Currently assumes /24 subnet
- DNS zones always correspond to a specific subnet
- Only tested on ubuntu 20.04
- No interface configuration
