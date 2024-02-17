# Simple Firewall for Docker

This project outlines a method to manage firewall rules for Docker containers on a Linux server, utilizing `iptables` and `ipset`. It allows for specifying which IP addresses are allowed to connect to specific ports.

## Configuration and Usage

### Dependencies

Ensure `iptables`, `ipset`, and `netfilter-persistent` are installed on your system.

### Creating and Populating the IP Set

Create an `ipset` named `allowed_ips`:

```bash
sudo ipset create allowed_ips hash:ip
```

IP addresses are specified in `/etc/ipset.rules`:

```bash
# Allowlist IP addresses
192.168.0.27
192.168.0.177
192.168.0.25
```

### Update Script

Use `/usr/local/bin/update-ipset.sh` to read and apply the IP addresses from `/etc/ipset.rules` to the `allowed_ips` set:

```bash
!/bin/bash

IPSET_LIST="allowed_ips"
CONFIG_FILE="/etc/ipset.rules"

# Flush the current list

ipset flush "$IPSET_LIST"


 Add each IP address to the ipset list

while IFS= read -r line; do

    if [[ -z "$line" || "$line" =~ ^# ]]; then continue; fi

    ipset add "$IPSET_LIST" "$line"

done < "$CONFIG_FILE"

```

Make it executable:


