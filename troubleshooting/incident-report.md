# Incident Report: Remote Clients Receiving APIPA Addresses

## Summary

Clients D1 and D2 on the '192.168.1.24/29' network could not obtain DHCP leases from the centralized DHCP server on R1. Both clients assigned themselves APIPA addresses.

## Expected behavior

R4 should receive DHCP broadcasts on its LAN-facing interface and relay them to R1 at '10.0.0.1'. R1 should identify the originating subnet from the relay-agent address and allocate a lease from the '192.168.1.24/29' pool.

## Evidence

- R4 WAN interface '10.0.0.10/30': up/up
- R4 LAN interface '192.168.1.25/29': up/up
- R4 DHCP relay destination: '10.0.0.1'
- R4–R3 OSPF neighbor: FULL
- R4 route toward '10.0.0.0/30': learned by OSPF inter-area
- R1 route to '192.168.1.24/29': learned by OSPF inter-area

These checks ruled out interface failure, missing relay configuration, broken OSPF adjacency, and missing return routing.

## Root cause

R1 contained two overlapping DHCP pools:

```cisco
ip dhcp pool Test4
 network 192.168.1.24 255.255.255.252

ip dhcp pool Test5
 network 192.168.1.24 255.255.255.248
```

The actual R4 LAN uses '/29'. The obsolete '/30' pool overlapped the correct scope and caused ambiguous pool selection.

## Corrective action

```cisco
configure terminal
no ip dhcp pool Test4
end
write memory
```

The clients were renewed after retaining only the correct `/29` pool.

## Validation

- D1 and D2 received addresses in '192.168.1.26 - 192.168.1.30'.
- The subnet mask was '255.255.255.248'.
- The default gateway was '192.168.1.25'.
- R1 displayed the leases with 'show ip dhcp binding'.
- OSPF adjacency and inter-area routes remained stable.

## Lessons learned

1. DHCP relay depends on bidirectional routing.
2. The server chooses a pool based on the relay-agent gateway address.
3. Overlapping DHCP scopes should be removed even if one appears unused.
4. A structured process helps separate Layer 1/2, routing, relay, and DHCP-server problems.
