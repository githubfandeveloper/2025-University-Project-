# Router-Based ACLs (Cloud-Hosted Servers)

---

## Network Subnet Legend

| Scope        | Name                 | CIDR / Range                  | Wildcard (ACL)       | Notes                                   |
|--------------|----------------------|-------------------------------|----------------------|-----------------------------------------|
| On-Prem VLAN | VLAN 10 – Student    | 10.86.96.0/26 (10.86.96.1–.62)| 10.86.96.0 0.0.0.63  | Default GW: 10.86.96.1                  |
| On-Prem VLAN | VLAN 20 – Staff      | 10.86.96.64/27 (10.86.96.65–.94)| 10.86.96.64 0.0.0.31| Default GW: 10.86.96.65                  |
| On-Prem VLAN | VLAN 30 – Management | 10.86.96.96/28 (10.86.96.97–.110)| 10.86.96.96 0.0.0.15| Default GW: 10.86.96.97                  |
| On-Prem VLAN | VLAN 40 – Guest      | 10.86.96.112/29 (10.86.96.113–.118)| 10.86.96.112 0.0.0.7| Default GW: 10.86.96.113                 |
| On-Prem VLAN | VLAN 50 – Admin      | 10.86.96.120/30 (10.86.96.121–.122)| 10.86.96.120 0.0.0.3| Default GW: 10.86.96.121                 |
| Cloud        | vm-subnet (CLOUD_NET)| 10.0.1.0/27 (10.0.1.4–10.0.1.30)| 10.0.1.0 0.0.0.31   | Azure reserves first 4 + last 1         |
| Cloud        | GatewaySubnet        | 10.0.1.32/27                   | —                    | Reserved for Azure VPN and Firewall     |

---

## ACL for Staff VLAN (VLAN 20) [Applied inbound on G0/0.20]

| Order | Action | Protocol | Source IP            | Destination IP      | Src Port | Dst Port | Description                                |
|-------|--------|----------|----------------------|---------------------|----------|----------|--------------------------------------------|
| 10    | Permit | ip       | 10.86.96.64 0.0.0.31 | 10.86.96.96 0.0.0.15| Any      | Any      | Allow Staff to on-prem Management portals  |
| 20    | Permit | tcp      | 10.86.96.64 0.0.0.31 | CLOUD_LMS_IP        | Any      | 80, 443  | Allow Staff to access Cloud LMS (HTTP/HTTPS)|
| 30    | Deny   | ip       | Any                  | 10.86.96.0 0.0.0.63 | Any      | Any      | Deny Staff access to Student VLAN          |
| 40    | Deny   | ip       | Any                  | 10.0.1.0 0.0.0.31   | Any      | Any      | Deny Staff access to other cloud servers   |
| 50    | Permit | ip       | Any                  | Any                 | Any      | Any      | Permit Internet/general traffic            |

---

## ACL for Student VLAN (VLAN 10) [Applied inbound on G0/0.10]

| Order | Action | Protocol | Source IP            | Destination IP      | Src Port | Dst Port | Description                                |
|-------|--------|----------|----------------------|---------------------|----------|----------|--------------------------------------------|
| 10    | Permit | tcp      | 10.86.96.0 0.0.0.63  | CLOUD_LMS_IP        | Any      | 80       | Allow Student HTTP access to cloud LMS     |
| 20    | Permit | tcp      | 10.86.96.0 0.0.0.63  | CLOUD_LMS_IP        | Any      | 443      | Allow Student HTTPS access to cloud LMS    |
| 30    | Deny   | ip       | Any                  | 10.86.96.64 0.0.0.31| Any      | Any      | Deny Student access to Staff VLAN          |
| 40    | Deny   | ip       | Any                  | 10.86.96.96 0.0.0.15| Any      | Any      | Deny Student access to Management VLAN     |
| 50    | Deny   | ip       | Any                  | 10.0.1.0 0.0.0.31   | Any      | Any      | Deny Student access to other cloud servers |
| 60    | Permit | ip       | Any                  | Any                 | Any      | Any      | Permit all other traffic                   |

---

## ACL for Guest VLAN (VLAN 40) [Applied inbound on G0/0.40]

| Order | Action | Protocol | Source IP | Destination IP        | Src Port | Dst Port | Description                                |
|-------|--------|----------|-----------|-----------------------|----------|----------|--------------------------------------------|
| 10    | Deny   | ip       | Any       | 10.86.96.0 0.0.0.63   | Any      | Any      | Deny Guest access to Student VLAN          |
| 20    | Deny   | ip       | Any       | 10.86.96.64 0.0.0.31  | Any      | Any      | Deny Guest access to Staff VLAN            |
| 30    | Deny   | ip       | Any       | 10.86.96.96 0.0.0.15  | Any      | Any      | Deny Guest access to Management VLAN       |
| 40    | Deny   | ip       | Any       | 10.86.96.120 0.0.0.3  | Any      | Any      | Deny Guest access to Admin VLAN            |
| 50    | Deny   | ip       | Any       | 10.0.1.0 0.0.0.31     | Any      | Any      | Deny Guest access to all cloud servers     |
| 60    | Permit | ip       | Any       | Any                   | Any      | Any      | Permit Guest Internet access               |

---

## ACL for Management VLAN (VLAN 30) [Applied inbound on G0/0.30]

| Order | Action | Protocol | Source IP            | Destination IP | Src Port | Dst Port | Description                                |
|-------|--------|----------|----------------------|----------------|----------|----------|--------------------------------------------|
| 10    | Permit | ip       | 10.86.96.96 0.0.0.15 | 10.0.1.0 0.0.0.31 | Any    | Any      | Allow Management full access to cloud      |
| 20    | Permit | ip       | 10.86.96.96 0.0.0.15 | 10.86.96.0 0.0.0.255 | Any | Any      | Allow Management to on-prem subnets        |
| 30    | Deny   | ip       | 10.86.96.96 0.0.0.15 | Any            | Any      | Any      | Optionally deny direct Internet             |
| 40    | Permit | ip       | Any                  | Any            | Any      | Any      | Permit remaining traffic                   |

---

## ACL for Admin VLAN (VLAN 50) [Applied inbound on G0/0.50]

| Order | Action | Protocol | Source IP             | Destination IP | Src Port | Dst Port | Description                                |
|-------|--------|----------|-----------------------|----------------|----------|----------|--------------------------------------------|
| 10    | Permit | ip       | 10.86.96.120 0.0.0.3  | CLOUD_DC_IP    | Any      | Any      | Allow Admin access to cloud DC/DNS         |
| 20    | Permit | ip       | 10.86.96.120 0.0.0.3  | CLOUD_JUMPBOX_IP | Any    | Any      | Allow Admin access to cloud bastion/jumpbox|
| 30    | Permit | ip       | 10.86.96.120 0.0.0.3  | [Router_IP]    | Any      | Any      | Allow Admin access to on-prem router       |
| 40    | Permit | ip       | 10.86.96.120 0.0.0.3  | [Firewall_IP]  | Any      | Any      | Allow Admin access to on-prem firewall     |
| 50    | Deny   | ip       | Any                   | 10.86.96.0 0.0.0.127 | Any | Any      | Deny Admin access to Student/Staff/Guest VLANs |
| 60    | Permit | ip       | Any                   | Any            | Any      | Any      | Permit other admin traffic                 |

---
