# LinkedIn Post

🚀 **CCNA Lab: Centralized DHCP Across a Multi-Area OSPF Network**

I recently completed a Cisco Packet Tracer lab focused on IPv4 subnetting, centralized DHCP, DHCP relay, and multi-area OSPF.

The topology included:

✅ OSPF Area 0 and Area 1  
✅ An Area Border Router  
✅ '/30' point-to-point links and '/29' LANs  
✅ A centralized DHCP server  
✅ DHCP relay using 'ip helper-address' 

During testing, clients on the remote '192.168.1.24/29' LAN received APIPA addresses. I used a structured troubleshooting process to verify interface status, the relay configuration, OSPF neighbors, inter-area routes, the return path, and DHCP pools.

The root cause was an obsolete '/30' DHCP pool overlapping the correct '/29' pool. After removing the incorrect scope and renewing the clients, they successfully received valid DHCP leases.

This lab reinforced an important lesson: in a routed environment, DHCP troubleshooting requires checking the complete path—not just the helper address.

📁 Full topology, corrected configurations, troubleshooting report, and verification checklist:

https://github.com/arpan3a/ccna-multiarea-ospf-dhcp-lab

#CCNA #Cisco #Networking #DHCP #OSPF #PacketTracer #NetworkEngineering #Troubleshooting #LearningInPublic
