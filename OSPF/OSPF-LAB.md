# OSPF LAB — FROM SCRATCH

## Topology

```text
                    R4
                    |
              10.0.24.0/30
                    |
                    R2
                  /    \
       10.0.12.0/30   10.0.23.0/30
                /        \
              R1----------R3
                 10.0.13.0/30
                |             |
               SW1           SW3
              /   \            |
            PC1   PC2         PC3
```

## IP Addressing

| Device | Interface | IP Address  | Subnet Mask     |
| ------ | --------- | ----------- | --------------- |
| R1     | G0/0      | 192.168.1.1 | 255.255.255.0   |
| R1     | G0/1      | 10.0.12.1   | 255.255.255.252 |
| R1     | G0/2      | 10.0.13.1   | 255.255.255.252 |
| R2     | G0/0      | 10.0.12.2   | 255.255.255.252 |
| R2     | G0/1      | 10.0.23.1   | 255.255.255.252 |
| R2     | G0/2      | 10.0.24.1   | 255.255.255.252 |
| R3     | G0/0      | 10.0.23.2   | 255.255.255.252 |
| R3     | G0/1      | 10.0.13.2   | 255.255.255.252 |
| R3     | G0/2      | 192.168.3.1 | 255.255.255.0   |

## R1 Configuration

```cisco
enable
configure terminal

hostname R1

interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit

interface g0/1
 ip address 10.0.12.1 255.255.255.252
 no shutdown
exit

interface g0/2
 ip address 10.0.13.1 255.255.255.252
 no shutdown
exit
```

## R2 Configuration

```cisco
enable
configure terminal

hostname R2

interface g0/0
 ip address 10.0.12.2 255.255.255.252
 no shutdown
exit

interface g0/1
 ip address 10.0.23.1 255.255.255.252
 no shutdown
exit

interface g0/2
 ip address 10.0.24.1 255.255.255.252
 no shutdown
exit
```

## R3 Configuration

```cisco
enable
configure terminal

hostname R3

interface g0/0
 ip address 10.0.23.2 255.255.255.252
 no shutdown
exit

interface g0/1
 ip address 10.0.13.2 255.255.255.252
 no shutdown
exit

interface g0/2
 ip address 192.168.3.1 255.255.255.0
 no shutdown
exit
```

## R4 Configuration

```cisco
enable
configure terminal

hostname R4

interface g0/0
 ip address 10.0.24.2 255.255.255.252
 no shutdown
exit
```

## OSPF Configuration

### R1

```cisco
router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0
 network 10.0.13.0 0.0.0.3 area 0
```

### R2

```cisco
router ospf 1
 router-id 2.2.2.2
 network 10.0.12.0 0.0.0.3 area 0
 network 10.0.23.0 0.0.0.3 area 0
 network 10.0.24.0 0.0.0.3 area 0
```

### R3

```cisco
router ospf 1
 router-id 3.3.3.3
 network 10.0.23.0 0.0.0.3 area 0
 network 10.0.13.0 0.0.0.3 area 0
 network 192.168.3.0 0.0.0.255 area 0
```

### R4

```cisco
router ospf 1
 router-id 4.4.4.4
 network 10.0.24.0 0.0.0.3 area 0
```

## Verification

```cisco
show ip interface brief
show ip ospf neighbor
show ip route ospf
show ip ospf
```

## Ping Tests

From R1:

```cisco
ping 10.0.23.2
ping 192.168.3.1
```

From PC1:

```text
ping 192.168.3.10
```

## OSPF Path

```text
PC1
 ↓
SW1
 ↓
R1
 ↓
R2
 ↓
R3
 ↓
SW3
 ↓
PC3
```

R1 also has a direct link to R3:

```text
R1 ───────── R3
     OSPF
     backup path
```

This allows OSPF to calculate an alternative path when the primary path is unavailable.

## Troubleshooting Commands

```cisco
show ip interface brief
show ip ospf neighbor
show ip route
show ip route ospf
show running-config
```
