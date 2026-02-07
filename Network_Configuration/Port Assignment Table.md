# Port Assignment Table – Router C921-4P & Switch 3560-CX

| Device           | Interface   | Connected To      | VLAN / Subnet           | Role / Notes                          |
|------------------|-------------|-------------------|-------------------------|---------------------------------------|
| **Router C921-4P** | Gi0/0/0.10 | Switch Gi0/13 (Trunk) | VLAN 10 – 10.86.96.0/26 | Student gateway (10.86.96.1)          |
| Router C921-4P   | Gi0/0/0.20  | Switch Gi0/13 (Trunk) | VLAN 20 – 10.86.96.64/27| Staff gateway (10.86.96.65)           |
| Router C921-4P   | Gi0/0/0.30  | Switch Gi0/13 (Trunk) | VLAN 30 – 10.86.96.96/28| Management gateway (10.86.96.97)          |
| Router C921-4P   | Gi0/0/0.40  | Switch Gi0/13 (Trunk) | VLAN 40 – 10.86.96.112/29| Guest gateway (10.86.96.113)         |
| Router C921-4P   | Gi0/0/0.50  | Switch Gi0/13 (Trunk) | VLAN 50 – 10.86.96.120/30| Admin gateway (10.86.96.121)         |
| Router C921-4P   | Gi0/0/1     | WAN/ISP uplink    | —                       | Internet connection (public IP)       |
| Router C921-4P   | Gi0/0/2     | —                 | —                       | Spare                                 |
| Router C921-4P   | Gi0/0/3     | —                 | —                       | Spare                                 |
| **Switch 3560-CX** | Gi0/1     | AP01              | Trunk (10,20,30,40,50)  | Wireless AP #1 uplink                 |
| Switch 3560-CX   | Gi0/2       | AP02              | Trunk (10,20,30,40,50)  | Wireless AP #2 uplink                 |
| Switch 3560-CX   | Gi0/3       | Staff PC 1        | VLAN 20                 | Staff                                 |
| Switch 3560-CX   | Gi0/4       | Staff PC 2        | VLAN 20                 | Staff                                 |
| Switch 3560-CX   | Gi0/5       | Staff PC 3        | VLAN 20                 | Staff                                 |
| Switch 3560-CX   | Gi0/6       | Staff PC 4        | VLAN 20                 | Staff                                 |
| Switch 3560-CX   | Gi0/13      | Router Gi0/0/0    | Trunk (10,20,30,40,50)  | Router-on-a-stick uplink              |
| Switch 3560-CX   | Gi0/14      | Admin PC          | VLAN 50                 | Admin port                            |
| Switch 3560-CX   | Gi0/7–12, Gi0/15–16 | —        | —                       | Spare / shutdown                      |
