# Security Lab
Welcome to my Security Lab! I'm using 0xBen or Ben Heaters guide for building this lab out in VirtualBox (Hypervisor). I am a beginner but I am hoping to gain experience with tools used in an enterprise environment. With this environment I will be able to practice penetration testing against a wide variety of targets, as well as detection in a SIEM. Within the lab all machines are run on private IP addresses only accesible from my network. You can read more about defined private IP addresses in the following documentation (RFC 1918): https://datatracker.ietf.org/doc/html/rfc1918

This is the link to the guide I will be Following https://benheater.com/building-a-security-lab-in-virtualbox/

##  Lab Architecture 

The lab is built around a central **pfSense firewall** that acts as a router, segmenting traffic between multiple virtual networks. This design mimics a corporate environment, allowing for realistic attack and defense scenarios. All components are hosted within **VirtualBox** on a single machine.


### Core Component: pfSense Firewall

The pfSense VM is the heart of the lab. It connects to the physical home network via a **Bridged WAN interface** to get internet access and manages all internal lab traffic through several dedicated internal networks.

* **WAN Interface:** Connects to your home network (`192.168.1.0/24`) for internet access.
* **LAN Interface:** The primary internal network for attacker machines.
* **ISOLATED, AD_LAB, VULN_EGRESS Interfaces:** Dedicated segments for specific functions, ensuring traffic from one lab segment doesn't spill into another.

---

### Network Segments 

Each network segment is configured as a separate "Internal Network" in VirtualBox and corresponds to a pfSense interface. This isolates traffic, which is a key security principle.

####  LAN Network (`10.0.0.0/24`)
This network serves as the primary "attacker/penetration testing" zone 

* **Purpose:** Launching attacks, general network scanning, and hosting initial access points.
* **VMs on this network:**
    * `10.0.0.2` - **Kali Linux:** The primary offensive security machine.
    * `10.0.0.13` - **Vulnhub-VM:** A vulnerable machine for practice.

---

####  AD_LAB Network (`10.6.6.0/24`)
This is an isolated segment intended for specific experiments. In this diagram, it's used for a classic vulnerable target.

* **Purpose:** To host highly vulnerable machines in a contained environment.
* **VMs on this network:**
    * `10.6.6.22` - **Metasploitable2:** A well-known, intentionally vulnerable Linux VM used for penetration testing training.

---

####  VULN_EGRESS Network (`10.80.80.0/24`)
This network simulates a corporate production environment containing core services like Active Directory and user workstations.

* **Purpose:** To practice Active Directory attacks and lateral movement.
* **VMs on this network:**
    * `10.80.80.2` - **Domain Controller:** A Windows Server VM running Active Directory Domain Services.
    * `10.80.80.43` - **Win10-Enterprise2:** A Windows 10 client machine joined to the domain.

---

####  SIEM & Management Network (`10.10.10.0/24`)
This network is dedicated to security monitoring and management. It hosts the Security Information and Event Management (SIEM) platform and is where network traffic from other segments is mirrored for analysis.

* **Purpose:** Defense, monitoring, and intrusion detection. This is the "Blue Team" headquarters.
* **VMs on this network:**
    * `10.10.10.3` - **Wazuh OVA with Suricata:** An all-in-one SIEM and Network Intrusion Detection System (NIDS). It collects logs and alerts on suspicious network traffic.
    * `10.10.10.11` - **Vulnhub-VM:** Another vulnerable machine.
    * `10.10.10.12` - **Win10-Enterprise1:** A second Windows 10 client.



