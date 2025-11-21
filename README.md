# Advanced BGP Policy Control Lab (GNS3)

This project demonstrates an advanced BGP design built on **GNS3**, focusing heavily on **policy-based control of route selection** across multiple Autonomous Systems.  
The lab includes **iBGP**, **eBGP**, **route reflectors**, and advanced attributes such as **Local Preference**, **MED**, **Cisco Weight**, **Next-Hop-Self**, and **route filtering**  using distribute-lists.

---

## 📌 Topology Diagram

<img width="1861" height="895" alt="bgp" src="https://github.com/user-attachments/assets/ad8bb446-e260-430e-887c-d7f86ca415c0" />


---

## 📘 Overview

This lab consists of **four BGP Autonomous Systems** exchanging routes using both **eBGP** and **iBGP** sessions. The main objective was to understand and manipulate BGP’s path selection process across multiple networks using attribute tuning and route reflection.

The network includes:

- **ASN 111** – Large internal AS running iBGP  
- **ASN 112** – External AS on the left  
- **ASN 113** – External AS on the right  
- **ASN 114** – External AS at the bottom  

Routers within ASN 111 run **iBGP** using a **Route Reflector** to avoid full mesh.  
External ASNs exchange routes using **eBGP** adjacency.

---

## 🧩 Key Concepts Practiced

### **1. eBGP Peering**
Each border router forms external BGP sessions with neighboring ASNs to exchange routes.  
This demonstrated:

- eBGP next-hop behavior  
- Direct interface peering  
- Handling multiple inbound/outbound paths  

---

### **2. iBGP Peering**
Routers inside ASN 111 use iBGP, with all sessions manually configured.  
Key concepts applied:

- iBGP does not advertise iBGP-learned routes  
- Loopback interfaces used for stable peering  
- Update-source loopback configuration  
- Next-hop-self required for eBGP-learned routes  

---

### **3. Route Reflector Design**
To avoid full-mesh iBGP requirements:

- **R5 is the Route Reflector (RR)**  
- **R2, R3, R4, R6, R7, R8 are the RR Clients**

The RR architecture ensures that routes learned from one client are sent to others without  
requiring direct iBGP peerings.

---

### **4. BGP Attribute Manipulation**
The key purpose of this lab was to influence BGP’s path selection through policy changes:

#### **Local Preference**
- Used inside ASN 111  
- Higher local preference → preferred outbound path  
- Applied to incoming routes to influence outbound routing

#### **MED (Metric)**
- Used between ASNs  
- Lower MED → preferred inbound path  
- Applied on links such as 13.0.0.0/8, 3.0.0.0/8, etc.

#### **Cisco Weight**
- Applied on selective routers within ASN 111  
- Weight is **local to the router only** and overrides all other attributes  
- Used for deterministic path selection on specific devices

---

### **5. Next-Hop-Self**
Applied on routers receiving eBGP routes so that iBGP peers inside ASN 111 had reachable next-hops.  
Used on routers R2, R3, R4, R6, R7, R8.

---

### **6. Route Filtering (Distribute-List)**
One specific objective was:

- **Block loopback prefixes 112.x.x.x and 121.x.x.x from reaching ASN 113 (R10)**

This was achieved using **BGP distribute-lists**, preventing certain networks from being advertised  
to external peers.

---

## 🔍 Troubleshooting / Validation Commands

### **Check BGP neighbors**
```
show ip bgp summary
show ip bgp neighbors
```

## Check advertised / received routes
```
show ip bgp neighbors <ip> advertised-routes
show ip bgp neighbors <ip> received-routes
show ip bgp
```
## Check attribute influence
```
show ip bgp
show ip route
```

## Verify next-hop-self
```
show running-config | include next-hop-self
```

## 🏁 Conclusion

- This lab provided hands-on experience with advanced BGP behaviors, including:
- Policy manipulation with Local Preference, MED, and Weight
- Route reflector implementation
- iBGP and eBGP peering
- Next-Hop-Self handling
- Route filtering using distribute-lists
- Multi-AS routing behavior
- BGP path selection tuning
The topology demonstrates realistic service-provider-level routing control using Cisco IOS and GNS3.
---
