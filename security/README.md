# Network Security – Firewall Rules & Segmentation Strategy

The homelab security model is implemented using a strict zone-based firewall architecture with [OPNsense](chatgpt://generic-entity?number=0).

Each network segment is isolated by default, and only explicitly required traffic is authorized following the principle of least privilege.

---

## Security Principles Applied

- Default deny between all internal networks  
- Explicit allow for required services only  
- IoT containment and isolation  
- Dedicated management network  
- Protected server infrastructure zone  
- Encrypted remote administration  

This mirrors enterprise zero-trust network design.

---

## C4 Network – Smart Home Core & Wi-Fi (192.168.4.0/24)

![C4 Firewall Rules](images/c4-rules.png)

### Allowed traffic
- DNS to internal resolver (port 53)  
- HTTPS to reverse proxy (port 443)  
- Access to specific internal services:
  - Plex
  - TrueNAS
  - Vaultwarden
  - Homebridge
  - Audiobookshelf

### Blocked
- All RFC1918 private networks (inter-LAN traffic)

### Internet
- Explicitly allowed

### Default
- Deny all other traffic

**Purpose:**  
Provide required functionality while fully isolating smart devices from infrastructure and management networks.

---

## Management Network (MOH – 192.168.1.0/24)

![MOH Firewall Rules](images/moh-rules.png)

### Allowed traffic
- Access to critical infrastructure services:
  - TrueNAS
  - Plex
  - Vaultwarden
  - Minecraft server
  - internal web services

### Blocked
- All inter-LAN private networks

### Internet
- Explicitly allowed

### Default
- Deny all other traffic

**Purpose:**  
Dedicated secure control plane for infrastructure administration.

---

## Server Network – Infrastructure Core (192.168.2.0/24)

![Servers Firewall Rules](images/servers-rules.png)

### Allowed traffic
- Full intra-server communication  
- Required internet access for updates and services  

### Blocked
- All inter-LAN access from user and IoT segments  

### Default
- Deny all other traffic

**Purpose:**  
Protect critical services while allowing internal high-performance communication.

---

## Maison Network – Secondary IoT Devices (192.168.3.0/24)

![Maison Firewall Rules](images/maison-rules.png)

### Allowed traffic
- DNS only  
- Internet access only  

### Blocked
- All private networks  
- No server access  
- No management access  

### Default
- Deny all other traffic

**Purpose:**  
Strict containment of low-trust IoT hardware to reduce attack surface.

---

## Remote Access Security

Secure remote connectivity is implemented using [WireGuard](chatgpt://generic-entity?number=1).

### Capabilities
- Encrypted access to management and server networks  
- No exposure of IoT segments by default  
- Controlled firewall permissions  

**Purpose:**  
Safe off-site infrastructure management without public service exposure.

---

## Security Benefits Achieved

- Strong segmentation between trust zones  
- Elimination of lateral movement risk  
- IoT attack surface containment  
- Protected management plane  
- Controlled infrastructure exposure  
- Clear traffic auditing  

---

## Real-World Skills Demonstrated

- Enterprise firewall rule design  
- Network micro-segmentation  
- Zero-trust access control  
- Secure VPN integration  
- Infrastructure hardening  
- Practical cybersecurity implementation  

---

## Summary

This firewall architecture transforms a homelab into an enterprise-style secure network environment, providing real-world experience in:

- security zoning  
- traffic control  
- risk reduction  
- operational stability  

It reflects industry best practices used in professional IT infrastructure.
