# Verification Checklist

## Interfaces

- [x] R4 'FastEthernet0/0' is up/up with '10.0.0.10/30'.
- [x] R4 'FastEthernet0/1' is up/up with '192.168.1.25/29'.
- [x] R1 'Serial0/0' is up/up with '10.0.0.1/30'.

```cisco
show ip interface brief
```

## OSPF

- [x] R4 has a FULL adjacency with R3.
- [x] R1 has a FULL adjacency with R2.
- [x] R1 learns '192.168.1.24/29' as an inter-area route.
- [x] R4 learns the R1 transit network as an inter-area route.

```cisco
show ip ospf neighbor
show ip route
```

## DHCP

- [x] The incorrect overlapping '/30' pool is removed.
- [x] The correct R4 scope is '192.168.1.24/29'.
- [x] The gateway address '192.168.1.25' is excluded.
- [x] R4 relays DHCP to '10.0.0.1'.
- [x] Remote clients receive addresses in '192.168.1.26–192.168.1.30'.

```cisco
show ip dhcp pool
show ip dhcp binding
```

## Client checks

Expected client settings:

```text
IPv4 address: 192.168.1.26–192.168.1.30
Subnet mask:  255.255.255.248
Gateway:      192.168.1.25
```

Capture and add final client screenshots or command output here before publishing if desired.
