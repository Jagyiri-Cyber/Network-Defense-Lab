# Network-Defense-Lab

# Cybersecurity Virtual Lab: Network Defense & Threat Detection

## Overview
![Lab structure](https://github.com/user-attachments/assets/83882626-e15a-4ba7-951a-b97317342405)

This project is a virtual cybersecurity lab developed to build and secure a fictional enterprise network for Morgan Maxwell Real Estate. The lab leverages OPNSense Firewall, Suricata IDS/IPS, and various network and system configurations to simulate real-world blue and red teaming scenarios. The project demonstrates essential skills in configuring network security, threat detection, high availability, and monitoring.


## Objectives
- Establish a secure network environment for Morgan Maxwell Real Estate, a fictional company.
- Configure a next-generation firewall using OPNSense.
- Implement and test IDS/IPS systems.
- Develop threat detection strategies for network reconnaissance attempts.
- Ensure high availability with a redundant firewall cluster for resilience.

## Project Outline

### 1. Initial Setup
**Tools:** VirtualBox, Kali Linux, OPNSense.

**Steps:**
- Install VirtualBox and create both Kali Linux and OPNSense virtual machines.
- **OPNSense Configuration:**
  - Set up with 8GB storage, 2GB RAM, and 2 processors for efficient performance.
  - Configure Network Adapters: NAT adapter for external connectivity and an Internal Network adapter for LAN traffic.
  - Assign static IPs: NAT IP as `10.0.2.254` and LAN IP as `10.200.200.254`.
  - Validate connectivity by pinging the OPNSense firewall from Kali to confirm network setup.

![Screenshot (77)](https://github.com/user-attachments/assets/81358a96-b093-478a-b7c3-b17997906374)

![Screenshot 2024-09-28 210727](https://github.com/user-attachments/assets/103799bd-daf3-4902-a270-07fbfb0f65e9)


### 2. Intrusion Detection and Custom Signature Creation
**Objective:** Monitor and detect reconnaissance attempts, specifically for stealthy scans like Nmap SYN scans targeting the firewall.

**Walkthrough:**
- **Install IDS/IPS on OPNSense:**
  - Enable Suricata on OPNSense, configure it to monitor LAN traffic, and activate Emerging Threat rule sets.
  - Enable SSH access and upload new IDS rule using Filezilla into OPNSense file system.
  - Install Custom rules in OPNSense using a basic XML document and setting up an HTTP server.
  - Integrate custom Suricata rules to detect Nmap scans and specific traffic patterns associated with reconnaissance.

![Screenshot 2024-10-18 103907](https://github.com/user-attachments/assets/9330d4c0-4dbd-4c70-811b-5d75db6c5b6b)

![Opnsense xml to download rules](https://github.com/user-attachments/assets/b734d660-7394-4fe9-b864-b3b36534089f)

![filezilla sftp connection to Opnsense](https://github.com/user-attachments/assets/20f55d3f-b755-482a-8e9e-5b69213128c5)

![setting up http server](https://github.com/user-attachments/assets/5762827b-8066-4a02-8959-3337992cbea1)



- **Custom Rule Creation:**
  - **Goal:** Detect SYN stealth scans targeting the firewall.
  - **Rule Code:**
    ```plaintext
    alert tcp $HOME_NET any -> 10.200.200.1 any (msg:"POSSIBLE NMAP SYN STEALTH SCAN"; flags:S; threshold:type threshold, track by_src, count 50, seconds 1; sid:1000001;)
    ```
  - **Explanation:**
    - **msg:** Provides a descriptive alert message.
    - **flags:** Focuses on SYN packets only, typical in stealth scans.
    - **threshold:** Limits the number of alerts for repeated attempts, avoiding false positives.
  
  - To test, execute an Nmap SYN scan using `nmap -sS 10.200.200.1` from Kali Linux, observing the alert in Suricata.
  - 
![nmap synstealth scan and result](https://github.com/user-attachments/assets/71c9e0d7-f0da-43be-8a3b-3994faeb413b)

![IDS Stealth Scan detection alert](https://github.com/user-attachments/assets/f629135e-c171-4f87-8d98-fb2a84ea90a4)


### 3. Proxy and Web Filtering Configuration
**Objective:** Configure a transparent proxy using Squid in OPNSense and apply a blacklist to control and monitor network access.

**Walkthrough:**
- **Transparent Proxy Setup:**
  - Configure OPNSense to intercept HTTP/HTTPS traffic transparently.
  - Enable port forwarding and redirect user traffic through Squid for inspection.

![Opnsense transparent proxy config](https://github.com/user-attachments/assets/8a0b3419-17bc-46fa-a26d-d8108dff1ffa)


- **Web Filtering:**
  - Upload a custom blacklist to OPNSense to restrict access to certain websites, like social media platforms. I used is collated and maintain by a department in The Université Toulouse. It is free to use and very effective.
  
**Result:** Any user attempting to access restricted sites will receive a filtered page indicating blocked content.

![Squid result](https://github.com/user-attachments/assets/4e743d45-f649-470a-8288-ef95cdf66964)



### 4. High Availability (HA) Configuration
**Objective:** Ensure network resilience by setting up firewall redundancy with automatic failover.

**Walkthrough:**
- Clone the primary OPNSense VM to create a backup firewall, ensuring configurations mirror each other.
- **Interfaces:**
  - Primary firewall: Assign LAN IP `10.200.200.251`.
  - Backup firewall: Assign LAN IP `10.200.200.252`.
  
- **CARP Configuration:**
  - Configure the Virtual IPs: LAN as `10.200.200.254`, WAN as `10.0.2.254`.
  - Set up synchronization between primary and backup firewalls using pfSync to mirror configuration changes.

- **Testing Failover:** Simulate a firewall outage, confirming that the backup takes over seamlessly, maintaining network connectivity.

## Technologies & Skills Demonstrated
- **Firewall & IDS/IPS:** Configured OPNSense with IDS/IPS rules for proactive security.
- **Threat Detection:** Designed and tested detection rules for network reconnaissance attempts.
- **High Availability:** Deployed CARP-based failover and pfSync for resilient firewall clustering.

## Future Enhancements
- Integrate the ELK stack for enhanced threat hunting and monitoring.
- Extend lab setup with Active Directory for user and policy management.

## Project Impact
This lab is designed to showcase a wide range of cybersecurity competencies, including network security, system hardening, and advanced threat detection and prevention—essential skills for securing real-world enterprise networks.
