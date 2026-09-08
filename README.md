# Cybersecurity Lab Setup Report — Week 1 (WK1-PM1)

**Created By:** Director Christ Evvert Lisangan  
**Module:** WK1-PM1 - Lab Setup VirtualBox and Kali Linux  
**Date:** September 8, 2026  

---

## 1. Summary

This report documents the implementation and verification of a lab environment built using Oracle VirtualBox and Kali Linux as specified in module WK1-PM1. The laboratory utilizes an isolated virtual subnet (`10.0.0.0/24`) configured with NATNetwork, allowing the attacker machine full outbound Internet accessibility while providing network communication across target virtual machines.

---

## 2. Environment Specifications

| Component | Configuration Parameter | Setting / Value |
| :--- | :--- | :--- |
| **Host System** | Base OS | Windows 11 (Host) |
| **Hypervisor** | Virtualization Platform | Oracle VirtualBox v7.2.16 |
| **Network Type** | Virtual Adapter Mode | NATNetwork (`CSInternNetwork`) |
| **Subnet Scope** | IPv4 Network Address | `10.0.0.0/24` |
| **DHCP Scope** | Automatic Address Allocation | `10.0.0.2` to `10.0.0.99 /24` *(Configured statically)* |
| **Attacker Machine** | Operating System | Kali Linux 2026.2 (64-bit) |
| **Kali Network** | Static IPv4 Address | `10.0.0.2 /24` |
| **Kali Routing** | Default Gateway | `10.0.0.1` |
| **DNS Resolution** | Primary DNS Server | `8.8.8.8`[cite: 2] |

---

## 3. Network Architecture Topology

```mermaid
graph TD
    subgraph MainHost["Main Host (Windows 11)"]
        subgraph VBox["VirtualBox 7.2.16"]
            subgraph NAT["NATNetwork (10.0.0.0/24) | Gateway: 10.0.0.1"]
                Kali["Kali Linux (Attacker)<br><b>10.0.0.2 /24</b>"]
                Switch(("Virtual Switch"))
                
                WinServer["Windows Server 2022<br><b>10.0.0.22 /24</b>"]
                Win11["Windows 11<br><b>10.0.0.11 /24</b>"]
                Win10["Windows 10<br><b>10.0.0.10 /24</b>"]
                Android["Android 9.0<br><b>10.0.0.9 /24</b>"]
                Win7["Windows 7<br><b>10.0.0.7 /24</b>"]

                Kali <--> Switch
                Switch <--> WinServer
                Switch <--> Win11
                Switch <--> Win10
                Switch <--> Android
                Switch <--> Win7
            end
        end
    end

    style MainHost fill:#111827,stroke:#4b5563,color:#fff
    style VBox fill:#1f2937,stroke:#6b7280,color:#fff
    style NAT fill:#374151,stroke:#9ca3af,stroke-dasharray: 5 5,color:#fff
    style Kali fill:#991b1b,stroke:#f87171,color:#fff
    style Switch fill:#4b5563,stroke:#d1d5db,color:#fff
    style WinServer fill:#1e3a8a,stroke:#93c5fd,color:#fff
    style Win11 fill:#1e40af,stroke:#60a5fa,color:#fff
    style Win10 fill:#1e40af,stroke:#60a5fa,color:#fff
    style Android fill:#15803d,stroke:#4ade80,color:#fff
    style Win7 fill:#1e40af,stroke:#60a5fa,color:#fff
