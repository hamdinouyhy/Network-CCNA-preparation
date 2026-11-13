# Day 21: OSPF Interface Configuration & Reference Bandwidth

##  Course Reference
This lab is part of **Jeremy's IT Lab** — CCNA 200-301 (Cisco Certified Network Associate) course.

##  Objective
Configure OSPF directly on router interfaces, adjust the OSPF reference bandwidth to accurately calculate metrics for FastEthernet links, and configure R1 as an ASBR to inject a default route into the OSPF domain. Additionally, analyze OSPF Hello packets using Simulation mode.

##  Network Topology

![Lab Topology](lab-screenshot.png)

##  Devices Used

| Device | Type | IP Address | Role |
|--------|------|------------|------|
| R1 | Router | 203.0.113.1/30, 10.0.12.1/30, 10.0.13.1/30, 1.1.1.1/32 (Lo) | ASBR / Edge Router |
| R2 | Router | 10.0.12.2/30, 10.0.24.2/30, 2.2.2.2/32 (Lo) | Internal OSPF Router |
| R3 | Router | 10.0.13.2/30, 10.0.34.1/30, 3.3.3.3/32 (Lo) | Internal OSPF Router |
| R4 | Router | 10.0.24.1/30, 10.0.34.2/30, 192.168.4.254/24, 4.4.4.4/32 (Lo) | Internal OSPF Router |
| ISPR1 | Router | 203.0.113.2/30 | ISP (Pre-configured) |
| SW1 | Switch | N/A | LAN Switch for PC1 |
| PC1 | Host | 192.168.4.1/24 | End User |

##  Configuration Steps

### Task 1 & 2: Basic Configuration and Loopbacks
*(Note: IP addresses must be assigned to physical interfaces as per the topology before proceeding).*

**On R1 (Example for Loopback):**

    configure terminal
    interface loopback 0
     ip address 1.1.1.1 255.255.255.255

### Task 3 & 4: OSPF Direct Interface Configuration & Reference Bandwidth
Enable OSPF directly on the interfaces, configure passive interfaces, and change the reference bandwidth so FastEthernet (100 Mbps) has a cost of 100 (Reference Bandwidth = 10,000 Mbps).

**On R1:**

    configure terminal
    router ospf 1
     ip ospf 1 area 0
     exit
    interface g0/0
     ip ospf 1 area 0
    interface f1/0
     ip ospf 1 area 0
    interface loopback 0
     ip ospf 1 area 0
    router ospf 1
     passive-interface loopback 0
     auto-cost reference-bandwidth 10000

**On R2:**

    configure terminal
    router ospf 1
     ip ospf 1 area 0
     exit
    interface g0/0
     ip ospf 1 area 0
    interface f1/0
     ip ospf 1 area 0
    interface loopback 0
     ip ospf 1 area 0
    router ospf 1
     passive-interface loopback 0
     auto-cost reference-bandwidth 10000

**On R3:**

    configure terminal
    router ospf 1
     ip ospf 1 area 0
     exit
    interface f1/0
     ip ospf 1 area 0
    interface f2/0
     ip ospf 1 area 0
    interface loopback 0
     ip ospf 1 area 0
    router ospf 1
     passive-interface loopback 0
     auto-cost reference-bandwidth 10000

**On R4:**

    configure terminal
    router ospf 1
     ip ospf 1 area 0
     exit
    interface f1/0
     ip ospf 1 area 0
    interface f2/0
     ip ospf 1 area 0
    interface g0/0
     ip ospf 1 area 0
    interface loopback 0
     ip ospf 1 area 0
    router ospf 1
     passive-interface loopback 0
     passive-interface g0/0
     auto-cost reference-bandwidth 10000

### Task 5: Configure R1 as ASBR (Default Route Injection)
**On R1:**

    configure terminal
    ip route 0.0.0.0 0.0.0.0 203.0.113.2
    router ospf 1
     default-information originate

##  Verification

| Test | Command | Expected Result |
|------|---------|-----------------|
| Verify OSPF Neighbors | `show ip ospf neighbor` | All adjacent routers should show a state of `FULL`. |
| Verify Interface Cost | `show ip ospf interface brief` | FastEthernet interfaces should show a cost of `100`. |
| Verify Default Route | `show ip route` (on R4) | An `O*E2` route for `0.0.0.0/0` is present in the table. |
| Hello Packet Analysis | Simulation Mode (Event List) | Observe the OSPF Hello packet fields (Router ID, Hello/Dead intervals, Area ID, etc.). |

##  What I Learned

- **Interface-level OSPF:** How to enable OSPF directly on an interface using `ip ospf <process-id> area <area-id>` instead of using the `network` command.
- **Reference Bandwidth:** How OSPF calculates cost and why adjusting the `auto-cost reference-bandwidth` is crucial in modern networks with links faster than 100 Mbps.
- **OSPF Packets:** Viewing actual OSPF Hello packets in Simulation mode to understand the fields required to form neighbor adjacencies.
- **ASBR Operations:** Injecting a default route using `default-information originate`.


---