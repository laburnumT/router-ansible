# ROUTER-SETUP

An ansible role for setting up a machine to run as a router with its own dns and dhcp capabilities.

</br>

## Configuration

Configuration of variables is done in the inventory file.
The script expects the inventory to be at inventory/hosts.yaml. A different location can be specified using the `-i` flag.

The inventory file works as a [regular ansible inventory file](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html), so I will only go over the specific variables per host. 

An example inventory file can be found at [inventory/hosts.yaml.example](inventory/hosts.yaml.example)

### Main

| Key 		| Required	| Default	| Description	|
|---		|---		|---		|---		|
| wan_interface	| yes		| -		| The interface to the "internet" |
| zones		| yes		| -		| List of zones managed by the dns server |


### Main.Zones[]

| Key 		| Required	| Default	| Description	|
|---		|---		|---		|---		|
| name	| yes | - | Domain name of the zone |
| lan_interface	| yes | - | The interface to the subnet the server is the router for |
| lan_ip | yes | - | First three octets for the subnet |
| dhcp_dynamic_range_bottom | yes | 100 | Lowest value available for dynamic dhcp |
| dhcp_dynamic_range_top | yes | 199 | Highest value available for dynamic dhcp |
| dns | yes | - | List of static ips in the zone |


### Main.Zones[].dns[]
| Key 		| Required	| Default	| Description	|
|---		|---		|---		|---		|
| hostname | yes | - | Hostname of the machine |
| ip | yes | - | Static ip of the machine |
| MAC | yes | - | MAC address of the interface of the machine |
| cnames | no | - | List of CNAMES associated with the machine |

</br>

## Run

### Requirements
`pip install -r requirements.txt`

### Deploy
`ansible-playbook router.yaml`

## Notes

- Currently assumes /24 subnet
- Only tested on ubuntu 20.04
- No interface configuration 
