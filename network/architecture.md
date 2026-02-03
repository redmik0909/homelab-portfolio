# Network Architecture – Segmented Multi-Interface Firewall Design

This homelab network was designed following enterprise-style segmentation principles to isolate services, IoT devices, user traffic, and management access.

The core of the network is built around a multi-interface firewall using [OPNsense] with dedicated physical interfaces mapped to managed switches.

---

## ISP Edge & WAN Configuration

The ISP-provided router (Bell) was kept only as a physical gateway.

OPNsense establishes a direct PPPoE connection to the ISP in order to:
- Obtain the public IP address directly on the firewall
- Bypass ISP routing limitations
- Maintain full control over NAT and security policies

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

## Management Network (MOH – 192.168.1.0/24)

This network hosts the primary administration workstation.

Purpose:
- Restricted access to infrastructure services
- Secure management plane similar to enterprise environments
- No exposure to IoT or user networks

---

## Server Network (192.168.2.0/24)

This segment hosts all core infrastructure services:

- Proxmox virtualization platform
- TrueNAS storage server
- Proxmox Backup Server
- Docker and LXC service hosts

Purpose:
- High-performance internal communication
- Complete isolation from IoT and user traffic
- Reduced attack surface

---

## C4 Network – Smart Home Core & Wi-Fi (192.168.4.0/24)

This segment includes:

- Wi-Fi infrastructure managed through [UniFi]
- Apple TV and primary client devices
- Smart home controllers using [Control4]

Purpose:
- Low latency for real-time automation systems
- Controlled traffic flow
- Isolation from servers and secondary IoT devices

---

## Maison Network – Secondary IoT (192.168.3.0/24)

This network hosts:

- Lighting hubs  
- Garage automation bridges  
- Miscellaneous smart devices  

Purpose:
- Containment of potentially insecure IoT hardware
- Prevention of lateral movement toward servers and admin systems

---

## Remote Access – WireGuard VPN

Secure remote connectivity is provided using [WireGuard].

Capabilities:
- Encrypted access to internal infrastructure
- Management from outside the home
- No public exposure of internal services

---

## Switching Infrastructure

Each internal network is connected to a dedicated managed switch directly mapped to a firewall interface.

Benefits:
- Physical and logical isolation
- Simplified traffic control
- Enterprise-style network layout

---

## Design Principles Applied

- Network segmentation by role
- Zero-trust style access control
- IoT isolation
- Dedicated management plane
- Secure remote administration
- Performance separation for infrastructure traffic

---

## Outcome

This design provides:

- Increased security
- Easier troubleshooting
- Enterprise-grade organization
- Real-world infrastructure experience

