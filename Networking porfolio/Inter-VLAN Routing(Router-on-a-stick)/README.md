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

## Switchport Config
### VLAN 10 (HR)
```cisco
enable
configure terminal

interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/9acbbd6576429aef92d238711cc35eb2c5747eb4/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/VLAN%2010%20Switchport.png)

### VLAN 20 (IT)
```cisco
interface range fastEthernet 0/3-4
switchport mode access
switchport access vlan 20
exit
end
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/a319687ca7b6ca691224262d965da6b5bcbdbc4f/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/VLAN%2020%20Switchport.png)

## Trunk Link Configuration
On Port Fa0/24 which connects the switch to the router
```cisco
enable
configure terminal

interface fastEthernet 0/24
switchport mode trunk
no shutdown
exit

end
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/d187c3a0c28c21aa334d12e336cbb72aef9d6ac0/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/Trunk%20Link%20config.png)

### Trunk Verification
```cisco
show interfaces trunk
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/2961caeaa31592a4f05b71cdabe39af876a7e88b/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/Trunk%20verification.png)

## Configuring Router-on-a-stick
### Router addressing

| Router interface | VLAN | IP address |
| :--- | :--- | :--- |
| G0/0.10 | 10 | 192.168.10.1/24 |
| G0/0.20 | 20 | 192.168.20.1/24 |

### Configuring subinterface for HR
```cisco
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/e0ed687b85359b23a9d0752d6340f0f54bb1193b/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/VLAN%2010%20subinterface.png)

### Note:
- g0/0.10 creates a logical subinterface.
- encapsulation dot1Q 10 associates it with VLAN 10.
- ip address assigns the HR default gateway.

### Configuring subinterface for IT
```cisco
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

end
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/7d6e90891761ebdd9b1f8ee81e05fa0c309ba8ca/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/VLAN%2020%20subinterface.png)

### Verify Configuration
```cisco
show ip interface brief
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/c70c86aa65949d0f5aa6e397493e803d66d8eb7d/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/IP%20interface.png)

## PC connectivity test
### Test 1: Same VLAN Configuration
PC1-HR to PC2-HR
```cisco
ping 192.168.10.11
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/aef6c373b5aed973f48d778b85a8d069064f3d5e/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/Ping%20PCHR1-PCHR2.png)

### Test 2: Default gateway
```cisco
ping 192.168.10.1
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/5450e80a726c01276a2d2b5442148ac08c2089f2/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/Ping%20Default%20Gateway.png)

### Test 3: Inter-VLAN communication
PC1-HR to PC1-IT
```cisco
ping 192.168.20.10
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/3d78474447e7475409497e37fdff7abff4331098/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/InterVlan%20communication.png)

## Verification and Testing
### Switch Verification
To inspect VLAN Membership and Trunk Configuration
```cisco
show vlan brief
show interfaces trunk
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/b99c8ed81a48b60c9430d5f6d2a8c785a5729fb9/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/VLAN%20verify.png)
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/b99c8ed81a48b60c9430d5f6d2a8c785a5729fb9/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/Trunk%20verification.png)

### Router Verification
```cisco
show ip interface brief
show ip route
```
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/462a7e88c6bdb5e00942bd9d37a1f1c1f97ea214/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/ip%20interface%20brief.png)
![alt text](https://github.com/UmarHassan843/IT-Portfolio/blob/462a7e88c6bdb5e00942bd9d37a1f1c1f97ea214/Networking%20porfolio/Inter-VLAN%20Routing(Router-on-a-stick)/ip%20route.png)
