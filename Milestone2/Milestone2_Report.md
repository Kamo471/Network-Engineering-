## 5. Troubleshooting & Implementation Refinements

### 5.1 Resolution of IP Addressing Overlap Conflict
During the initial transition from the theoretical design (Milestone 1) to the active simulation phase in Cisco Packet Tracer (Milestone 2), an IP addressing overlap conflict was identified and resolved. 

* **The Problem:** The initial Milestone 1 routing plan allocated `192.168.39.128/26` for Future Expansion (VLAN 80), which spans from `.128` to `.191`. The point-to-point transit links between the Core Switch and Edge Gateways were originally assigned to `.160/30` and `.164/30`, falling directly inside the expansion range and causing Cisco Packet Tracer to reject the interface configurations.
* **The Impact:** Inter-VLAN traffic routing to the edge firewalls failed due to overlapping broadcast domains.
* **The Resolution:** Shifted the point-to-point transit subnets upward into the unallocated `/24` space starting at `.192`. 

The corrected infrastructure network addressing scheme has been successfully deployed as follows:

| Link / Function | Subnet Address | Subnet Mask | Assignable Host Range | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **Future Expansion (VLAN 80)** | 192.168.39.128/26 | 255.255.255.192 | 192.168.39.130 – 192.168.39.190 | 192.168.39.129 |
| **Transit Link (Core-to-R1)** | 192.168.39.192/30 | 255.255.255.252 | 192.168.39.193 – 192.168.39.194 | N/A |
| **Transit Link (Core-to-R2)** | 192.168.39.196/30 | 255.255.255.252 | 192.168.39.197 – 192.168.39.198 | N/A |

### 5.2 Updated Static Edge Route Configurations
To reflect the resolved transit network addresses, the edge routing engine parameters on the centralized **CORE SWITCH** were updated to execute the CR15 floating static routes:

* **Primary Route Configuration:** 
  `ip route 0.0.0.0 0.0.0.0 192.168.39.194` (Points to R1 - Metric 1)
* **Floating Backup Route Configuration:** 
  `ip route 0.0.0.0 0.0.0.0 192.168.39.198 10` (Points to R2 - Metric 10)

