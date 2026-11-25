# PureDrop Dairy: Dual-Provider Edge Network (Task 1)

Task 1 of 3: VLSM subnet plan and a basic two-LAN topology in Cisco Packet Tracer.
Later tasks add VLANs, OSPF, HSRP and dual-provider eBGP.

## Subnet plan (VLSM)
Full table in [`subnet_plan.csv`](subnet_plan.csv). Subnets are allocated largest first so no address space is wasted.

| Site | Subnet | Network | Mask | Usable range | Broadcast |
|---|---|---|---|---|---|
| HQ | HQ-Office (100 hosts) | 10.10.0.0/25 | 255.255.255.128 | 10.10.0.1 - 10.10.0.126 | 10.10.0.127 |
| HQ | HQ-Servers (25 hosts) | 10.10.0.128/27 | 255.255.255.224 | 10.10.0.129 - 10.10.0.158 | 10.10.0.159 |
| HQ | HQ-Mgmt (10 hosts) | 10.10.0.160/28 | 255.255.255.240 | 10.10.0.161 - 10.10.0.174 | 10.10.0.175 |
| Remote | Collection-LAN (50 hosts) | 10.20.0.0/26 | 255.255.255.192 | 10.20.0.1 - 10.20.0.62 | 10.20.0.63 |
| Remote | IoT-Sensors (25 hosts) | 10.20.0.64/27 | 255.255.255.224 | 10.20.0.65 - 10.20.0.94 | 10.20.0.95 |

HQ-Office and Collection-LAN are built in Packet Tracer in this task. The other subnets are planned for later tasks.

## Topology
```
PC1 --- PD-SW1 --- Gi0/0 [PD-R1] Gi0/1 --- PD-SW2 --- PC2
        (HQ LAN)                          (Remote LAN)
```
Add a Packet Tracer screenshot here: `docs/topology.png`

## Addressing
| Device | Interface | IP address | Mask | Gateway |
|---|---|---|---|---|
| PD-R1 | Gi0/0 | 10.10.0.1 | 255.255.255.128 | n/a |
| PD-R1 | Gi0/1 | 10.20.0.1 | 255.255.255.192 | n/a |
| PC1 | NIC | 10.10.0.10 | 255.255.255.128 | 10.10.0.1 |
| PC2 | NIC | 10.20.0.10 | 255.255.255.192 | 10.20.0.1 |

## Routing
Both LANs are directly connected to PD-R1, so the router learns both networks as connected routes. No static route is needed for this task.

## Files
- `puredrop_basic.pkt`: Packet Tracer project
- `subnet_plan.csv`: subnet plan
- `configs/`: device configurations
- `outputs/ping_headoffice.txt`: ping results between PC1 and PC2

## Test
From PC1: `ping 10.20.0.10`. From PC2: `ping 10.10.0.10`. Results are in `outputs/ping_headoffice.txt`.
(The first ping may time out while ARP resolves; this is normal.)
