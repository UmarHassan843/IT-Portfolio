# Inter-VLAN Routing (Router-on-a-stick Configuration)
A company has two departments sharing one physical switch. The departments must be separated into different VLANs but still communicate through a router.

## Objectives
- Create and name VLANs.
- Assign switch ports to VLANs.
- Configure trunk links.
- Configure router-on-a-stick.
- Configure 802.1Q encapsulation.
- Verify inter-VLAN communication.
- Troubleshoot VLAN and trunking problems.

## Topology
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/cdc9c97b7ef87637ffdd9c91c05f1fc2f4414398/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/Topology.png)
### Devices
- 1 × Cisco 2911 router
- 1 × Cisco 2960 switch 
- 4 × PCs

## IP Addressing Plan And Connections
### Connections

| From | To |
| :--- | :--- |
| R1 G0/0 | SW1 Fa0/24 |
| PC-HR1 FastEthernet0 | SW1 Fa0/1 |
| PC-HR2 FastEthernet0 | SW1 Fa0/2 |
| PC-IT1 FastEthernet0 | SW1 Fa0/3 |
| PC-IT2 FastEthernet0 | SW1 Fa0/4 |

### PC Addressing

| Device | VLAN | IP address | Gateway |
| :--- | :--- | :--- | :--- |
| PC1 — HR | 10 | 192.168.10.10/24 | 192.168.10.1 |
| PC2 — HR | 10 | 192.168.10.11/24 | 192.168.10.1 |
| PC3 — IT | 20 | 192.168.20.10/24 | 192.168.20.1 |
| PC4 — IT | 20 | 192.168.20.11/24 | 192.168.20.1 |

### VLAN Plan

| VLAN | Name | Switch ports |
| :--- | :--- | :--- |
| 10 | HR | Fa0/1–Fa0/2 |
| 20 | IT | Fa0/3–Fa0/4 |
| Trunk | Router link | Fa0/24 |

### Switchport Plan

| Switch port | Connected device | VLAN |
| :--- | :--- | :--- |
| Fa0/1 | PC-HR1 | 10 |
| Fa0/2 | PC-HR2 | 10 |
| Fa0/3 | PC-IT1 | 20 |
| Fa0/4 | PC-IT2 | 20 |


## VLAN Configurations
### VLAN 10 (HR)
```cisco
vlan 10
name HR
exit
```
### VLAN 20 (IT)
```cisco
vlan 20
name IT
exit
```
### Verify VLANs
```cisco
end
show vlan brief
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/b4bd6132ada6f21aa99637738f8e05598101d273/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/VLAN%20verify.png)

### Switchport Config
