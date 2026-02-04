# Network Architecture – Segmented Multi-Interface Firewall Design

This homelab network was designed following enterprise-style segmentation principles to isolate services, IoT devices, user traffic, and management access.

The core of the network is built around a multi-interface firewall using OPNsense with dedicated physical interfaces mapped to managed switches.

---

## ISP Edge & WAN Configuration

The ISP-provided router (Bell) was kept only as a physical gateway.

OPNsense establishes a direct PPPoE connection to the ISP in order to:

- obtain the public IP address directly on the firewall  
- bypass ISP routing limitations  
- maintain full control over NAT and security policies  

---

## Internal Network Segmentation

Instead of a flat LAN, the infrastructure is divided into multiple security zones.

| Network Name | Subnet | Purpose |
|-------------|--------|--------|
| MOH (Management) | 192.168.1.0/24 | Secure admin workstation |
| Servers | 192.168.2.0/24 | Infrastructure & virtualization |
| Maison (IoT secondary) | 192.168.3.0/24 | Smart devices & hubs |
| C4 (Core smart home & Wi-Fi) | 192.168.4.0/24 | UniFi APs, Control4, main devices |
| WireGuard VPN | Dedicated interface | Secure remote access |

---

## DHCP & Addressing Strategy

Each internal network interface operates its own dedicated DHCP service to simplify client management and maintain clean segmentation.

### Exception – Server Network (192.168.2.0/24)

This segment uses **manually assigned static IP addresses** for:

- infrastructure services  
- containers and virtual machines  
- storage servers  
- core systems  

This provides:

- predictable service addressing  
- simplified firewall policies  
- easier troubleshooting  
- enterprise-style infrastructure management  

---

## Centralized DNS Architecture

All networks rely on a centralized internal DNS resolver.

Purpose:

- unified name resolution across all segments  
- consistent client behavior  
- centralized filtering and control  

(Detailed DNS security and filtering mechanisms are documented in the Services section.)

---

## Management Network – MOH (192.168.1.0/24)

This network hosts the primary administration workstation.

Purpose:

- restricted access to infrastructure services  
- dedicated secure management plane  
- no exposure to IoT or general user traffic  

---

## Server Network (192.168.2.0/24)

This segment hosts all core infrastructure services:

- virtualization platform  
- centralized storage servers  
- backup systems  
- containerized service workloads  

Purpose:

- high-performance internal communication  
- strict isolation from IoT and user devices  
- reduced attack surface  

---

## C4 Network – Smart Home Core & Wi-Fi (192.168.4.0/24)

This segment includes:

- wireless access points  
- primary client devices  
- smart home automation controllers  

Purpose:

- low latency for automation systems  
- controlled traffic flows  
- isolation from server infrastructure  

---

## Maison Network – Secondary IoT (192.168.3.0/24)

This network hosts:

- lighting hubs  
- garage automation bridges  
- various smart devices  

Purpose:

- containment of potentially insecure hardware  
- prevention of lateral movement toward critical systems  

---

## Remote Access – WireGuard VPN

Secure remote connectivity provides:

- encrypted access to internal infrastructure  
- safe management from external networks  
- no direct public exposure of services  

---

## Switching Infrastructure

Each internal network is connected to a dedicated managed switch directly mapped to a firewall interface.

Benefits:

- physical and logical isolation  
- simplified traffic control  
- enterprise-style network layout  

---

## Design Principles Applied

- role-based network segmentation  
- dedicated DHCP per zone  
- static addressing for infrastructure  
- centralized DNS  
- IoT isolation  
- secure remote administration  
- performance separation  

---

## Outcome

This architecture delivers:

- increased security  
- predictable service management  
- simplified troubleshooting  
- enterprise-grade organization  
- real-world network design experience  
