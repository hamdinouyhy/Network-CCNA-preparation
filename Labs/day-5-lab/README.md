# Day 05: MAC Address Table & ARP Analysis

## Course Reference
- Course: Jeremy's IT Lab - CCNA 200-301
- Topic: MAC Address Table, ARP, and Switch Learning

## Objective
Analyze how switches learn MAC addresses, how ARP works, and clear the MAC address table.

## Network Topology
![Lab Topology](lab-day-3-screenshot.png)

## Devices Used
| Device | Type | IP Address |
|--------|------|------------|
| PC1 | Generic PC | 192.168.1.1/24 |
| PC2 | Generic PC | 192.168.1.2/24 |
| PC3 | Generic PC | 192.168.1.3/24 |
| PC4 | Generic PC | 192.168.1.4/24 |
| SW1 | Cisco Switch | - |
| SW2 | Cisco Switch | - |

## Steps & Analysis

### Step 1: Ping from PC1 to PC3
Question: What messages will be sent over the network, and which devices will receive them?
Answer:
1. PC1 sends an ARP Request (broadcast) asking for PC3's MAC address.
2. All devices in the broadcast domain (PC2, PC3, PC4, SW1, SW2) receive the ARP Request.
3. PC3 replies with an ARP Reply (unicast) containing its MAC address.
4. PC1 sends ICMP Echo Request to PC3.
5. PC3 replies with ICMP Echo Reply.

### Step 2: Verify with Simulation Mode
- Switched to Simulation Mode.
- Sent ping from PC1 to PC3.
- Observed ARP Request, ARP Reply, ICMP Echo Request, and ICMP Echo Reply.

### Step 3: Generate Network Traffic
- Pinged between all PCs to allow switches to learn MAC addresses.
- Each ping causes the switch to record the source MAC address and the port it was received on.

### Step 4: View MAC Address Table
On SW1:
show mac address-table

On SW2:
show mac address-table

### Step 5: Clear Dynamic MAC Addresses
On SW1:
clear mac address-table dynamic

On SW2:
clear mac address-table dynamic

## What I Learned
1. MAC Address Table: Switches use it to forward frames to the correct port.
2. Learning Process: Switches learn source MAC addresses automatically.
3. ARP: Used to map IP addresses to MAC addresses.
4. Broadcast vs Unicast: ARP Request is broadcast, ARP Reply is unicast.
5. Clearing MAC Table: clear mac address-table dynamic removes dynamic entries.

---