# HomeLab
This repository is to track and document the progress of my own HomeLab that I will use for cybersecurity and sysadmin practice. This will be done in a simulated and isolated environment with two Virtual Machines: Ubuntu (the defender) and Kali Linux (the attacker)

---

## Virtual Machine Setup

- **Attacker:** Kali Linux
- **Defender:** Ubuntu Linux

Each VM uses:
- **NAT Adapter** – Provides internet access.
- **Host-Only Adapter** – Enables internal communication between VMs on a private, isolated network.

Bridged mode was not possible due to Wi-Fi limitations in my apartment.

---

## Services on Ubuntu (Defender)

The Ubuntu VM runs the following stack to simulate vulnerable web applications:

- **Apache** – Web server
- **PHP** – Backend processing
- **MySQL** – Database service
- **DVWA** – Damn Vulnerable Web Application

---

## Network & Traffic Flow

1. **Kali VM** attacks the **Ubuntu VM** via the Host-Only network.
2. **DVWA** is served on the Ubuntu machine using Apache and PHP, with data stored in MySQL.
3. Both VMs can access the internet via their NAT adapters (e.g., to install packages or tools).

---

## Network Diagram
This diagram shows the NAT and Host-Only adapter configuration between the Kali (attacker) and Ubuntu (defender) VMS.

![Network Diagram](flowcharts/ND.PNG)


---

## Flow Chart
This chart shows the services running inside the Ubuntu VM and how Kali interacts with DVWA.

![Network Diagram](flowcharts/VMconfig.PNG)



---

## Future Plans
- Add firewall (`SafeLine`), intrusion detection, and log monitoring tools.
