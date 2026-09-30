CMPG 325 — MILESTONE 1: CLIENT DESIGN REVIEW
• Project ID: CMPG325-2026-078
• Client ID: CLI-078
• Client: Kgosiemang & Associates Attorneys (Mahikeng)
• Industry: Legal Services
• Assigned IPv4 Address Block: 192.168.39.0/24
1. Client Requirements Analysis & Assumptions
1.1 Project Objective
To engineer a secure, highly resilient, and scalable hierarchical campus network infrastructure in Cisco Packet Tracer for Kgosiemang & Associates Attorneys. The environment must seamlessly handle immediate data traffic requirements while fully integrating redundant path structures and local wireless extensions.
1.2 Core Parameters Matrix
• Assigned Networking Challenge: Wireless LAN (AP integration & coverage) using standalone access layer extensions.
• Design Constraint: Allocation of a dedicated sub-infrastructure block to fully accommodate an entire new department being added next year.
• Change Request (CR15): Dual-homed network edge design integrating a secondary Internet service connection for active backup resilience and automated path failover.
1.3 Strategic Architectural Assumptions
• Departmental Separation: The business is structured around five primary organizational groups: Management/Partners, Attorneys, Administration, Finance, and Reception.
• Infrastructure Anchors: A localized, secure Datacenter environment contains an internal database server (Local-Server) and shared production print equipment (Network-Printer).
• Mobility Profiles: Internal legal practitioners utilize wireless enterprise laptops (Wireless-Laptop) to facilitate mobility between workspace boundaries.
2. Physical Topology Design Report
The hardware topology implements a three-tier hierarchical campus model designed to decouple failure domains and deliver wire-speed local switching performance.
2.1 Hardware Infrastructure Inventory Spec Sheet
• Central Routing Backbone: 1 x Cisco Catalyst 3650 Multilayer Switch (CORE_SWITCH). This device serves as the network's high-speed core and distribution engine, terminating all internal departmental Switched Virtual Interfaces (SVIs) to run hardware-based inter-VLAN routing.
• Enterprise Edge Gateways: 2 x Cisco ISR 4331 Routers (R1 and R2). Arranged as distinct upstream border nodes to satisfy change request CR15.
• Access Layer Hubs: 3 x Cisco Catalyst 2960 Switches (SW1, SW2, SW3). Deployed across the facility floor to isolate raw physical attachment points.
• Wireless Media Bridge: 1 x Access Point-PT (AP0). Functioning as a pure layer 2 over-the-air bridge to extend wireless network reach to mobile clients.
<img width="707" height="613" alt="image" src="https://github.com/user-attachments/assets/2b51ad1a-530e-459e-ab93-d904881a03dc" />
3. Logical Topology & Routing Architecture
3.1 VLAN Allocation Strategy
The network domain is strictly divided into functional broadcast environments to ensure strict traffic isolation between legal desks, financial databases, and unauthenticated wireless devices:
• VLAN 10 (Management): Firm executives and partner assets.
• VLAN 20 (Attorneys): Confidential client legal desktop systems.
• VLAN 30 (Administration): Back-office infrastructure and support channels.
• VLAN 40 (Finance): Highly secure processing nodes for billing structures.
• VLAN 50 (Reception): Front desk reception elements and network peripherals.
• VLAN 60 (Local Servers): Localized Datacenter hosting central information repositories.
• VLAN 70 (Wireless LAN): Target infrastructure for the standalone wireless extension challenge.
• VLAN 80 (Future Expansion Constraint): Subnet block set aside to seamlessly absorb next year's new department without requiring re-addressing.
3.2 Resilience and Border Routing Strategy (CR15)
To integrate the secondary upstream link without creating routing loops or asymmetric paths, edge boundaries are defined via point-to-point transit blocks. Internal inter-VLAN operations occur at the core switch.
Traffic exiting to external networks relies on a deterministic Floating Static Routing policy configured directly on the Multilayer Core Switch:
• Primary Path: A default static route pointing toward edge router R1 (192.168.39.194) with an administrative distance of 1.
• Resilient Floating Backup Path: A default static route pointing toward edge router R2 (192.168.39.198) configured with a higher administrative distance of 10.
4. Optimized VLSM IP Addressing Plan
The assigned 192.168.39.0/24 address block has been carefully engineered using Variable Length Subnet Masking (VLSM). All subnets are assigned based on host requirements while strictly avoiding overlaps and keeping a contiguous block of space available for future modifications.
4.1 Master VLSM Subnet Table

| VLAN | Function / Department | Host Capacity | Subnet Address | Subnet Mask | Usable Host Range | Default Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **10** | Management | 10 | `192.168.39.0/28` | `255.255.255.240` | `192.168.39.2 – 192.168.39.14` | `192.168.39.1` |
| **20** | Attorneys | 25 | `192.168.39.16/27` | `255.255.255.224` | `192.168.39.18 – 192.168.39.46` | `192.168.39.17` |
| **30** | Administration | 8 | `192.168.39.48/28` | `255.255.255.240` | `192.168.39.50 – 192.168.39.62` | `192.168.39.49` |
| **40** | Finance | 12 | `192.168.39.64/28` | `255.255.255.240` | `192.168.39.66 – 192.168.39.78` | `192.168.39.65` |
| **50** | Reception | 5 | `192.168.39.80/28` | `255.255.255.240` | `192.168.39.82 – 192.168.39.94` | `192.168.39.81` |
| **60** | Local Servers | 2 | `192.168.39.96/28` | `255.255.255.240` | `192.168.39.98 – 192.168.39.110` | `192.168.39.97` |
| **70** | Wireless LAN | 5 | `192.168.39.112/28` | `255.255.255.240` | `192.168.39.114 – 192.168.39.126` | `192.168.39.113` |
| **80** | Future Expansion | 50 | `192.168.39.128/26` | `255.255.255.192` | `192.168.39.129 – 192.168.39.190` | `192.168.39.129` |
| **—** | **Transit Core–R1** | **2** | **`192.168.39.192/30`** | **`255.255.255.252`** | **`192.168.39.193 – 192.168.39.194`** | `N/A` |
| **—** | **Transit Core–R2** | **2** | **`192.168.39.196/30`** | **`255.255.255.252`** | **`192.168.39.197 – 192.168.39.198`** | `N/A` |


