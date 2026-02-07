## Name: vm-subnet

Address range: 10.0.1.0/27   (32 IPs, 27 usable). Minus 5 reserved (Azure always reserves the first 4 + last 1) That will safely host VMs with breathing room

This gives usable IPs 10.0.1.4 – 10.0.1.30.

## Name: GatewaySubnet

Address range: 10.0.1.32/27   (32 IPs, 27 usable). Azure requires GatewaySubnet = /27 or larger

This gives usable IPs 10.0.1.36 - 10.0.1.62

## Plan

Replace Exchange server by Microsoft 365 Exchange Online, which is cheaper, safer, and easier.

