# Cybersecurity Lab Setup Report — Week 1 (WK1-PM1)

**Created By:** Christ Evvert Lisangan  
**Module:** WK1-PM1 - Lab Setup VirtualBox and Kali Linux  
**Date:** September 8, 2026  

---

## 1. Summary

This report documents the implementation and verification of a lab environment built using Oracle VirtualBox and Kali Linux as specified in module WK1-PM1[cite: 2]. The laboratory utilizes an isolated virtual subnet (`10.0.0.0/24`) configured with NATNetwork, allowing the attacker machine full outbound Internet accessibility while providing network communication across target virtual machines[cite: 2].

---

## 2. Environment Specifications

| Component | Configuration Parameter | Setting / Value |
| :--- | :--- | :--- |
| **Host System** | Base OS | Windows 11 (Host)[cite: 2] |
| **Hypervisor** | Virtualization Platform | Oracle VirtualBox v7.2.16[cite: 2] |
| **Network Type** | Virtual Adapter Mode | NATNetwork (`CSInternNetwork`)[cite: 2] |
| **Subnet Scope** | IPv4 Network Address | `10.0.0.0/24`[cite: 2] |
| **DHCP Scope** | Automatic Address Allocation | `10.0.0.2` to `10.0.0.99 /24` *(Configured statically)*[cite: 2] |
| **Attacker Machine** | Operating System | Kali Linux 2026.2 (64-bit)[cite: 2] |
| **Kali Network** | Static IPv4 Address | `10.0.0.2 /24`[cite: 2] |
| **Kali Routing** | Default Gateway | `10.0.0.1`[cite: 2] |
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

```

---

## 4. Step-by-Step Configuration Procedures

### Phase 1: VirtualBox Global Network Configuration

1. Navigated to **VirtualBox Manager > Tools > Network > NAT Networks**.


2. Created a new NAT Network named **`CSInternNetwork`**.


3. Configured the IPv4 Prefix to **`10.0.0.0/24`** and enabled DHCP Server.



### Phase 2: Kali Linux VM Network Setup

1. Opened Kali Linux VM **Settings > Network > Adapter 1**.


2. Attached the adapter to **NAT Network** and selected **`CSInternNetwork`**.


3. Set Promiscuous Mode to **Allow All**. This setting allows the virtual NIC to accept all packets visible on the virtual switch, including inter-VM traffic, host traffic, and spoofed frames.



---

## 5. Verification & Testing

### Verification Commands

Executed from the Kali Linux terminal to verify Internet connectivity and local host reachability:

```bash
# Test public Internet reachability
ping google.com

# Test connectivity to internal lab hosts
ping 10.0.0.9   # Android OS
ping 10.0.0.10  # Windows 10 Target VM

```

---

## 6. Screenshots & Evidence

### Screenshot 1: VirtualBox NAT Network Configuration (10.0.0.0/24)
<img width="959" height="503" alt="screenshot1-natnetwork" src="https://github.com/user-attachments/assets/58c3528a-d9d2-4e8c-8701-fd5f8da18228" />


### Screenshot 2: Kali Linux Network Settings & Static IP (10.0.0.2)
<img width="359" height="286" alt="screenshot2-kali-ip" src="https://github.com/user-attachments/assets/031c6392-673a-46cf-8425-f5e4a6504fe5" />


### Screenshot 3: Terminal Ping Test & Internet Connectivity Verification

<img width="346" height="120" alt="screenshot3-ping-google" src="https://github.com/user-attachments/assets/68369d6f-69e5-435c-bc66-220ea5b42a72" />

### Screenshot 4: Terminal Ping Test to Android OS (10.0.0.9)
<img width="260" height="105" alt="screenshot4-ping-android" src="https://github.com/user-attachments/assets/0536254e-ea50-4197-a9d6-7aeb9bbd8397" />


### Screenshot 5: Terminal Ping Test & Firewall Troubleshooting (Windows 10)
<img width="311" height="260" alt="screenshot5-win10-firewall" src="https://github.com/user-attachments/assets/cebf8e5f-7b7f-441f-a8c3-8ffce0e9d4ad" />


Initial test resulted in 100% packet loss due to Windows Defender Firewall dropping ICMP requests. By disabling the Windows Defender Firewall, the issue was resolved ping is successful.

```

```
