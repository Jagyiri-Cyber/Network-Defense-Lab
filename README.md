# Network-Defense-Lab

# Cybersecurity Virtual Lab: Network Defense & Threat Detection

## Overview
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
  - Assign static IPs: NAT IP as `10.0.2.x` and LAN IP as `10.200.200.x`.
  - Validate connectivity by pinging the OPNSense firewall from Kali to confirm network setup.

### 2. Intrusion Detection and Custom Signature Creation
**Objective:** Monitor and detect reconnaissance attempts, specifically for stealthy scans like Nmap SYN scans targeting the firewall.

**Walkthrough:**
- **Install IDS/IPS on OPNSense:**
  - Enable Suricata on OPNSense, configure it to monitor LAN traffic, and activate Emerging Threat rule sets.
  - Integrate custom Suricata rules to detect Nmap scans and specific traffic patterns associated with reconnaissance.

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

### 3. Proxy and Web Filtering Configuration
**Objective:** Configure a transparent proxy using Squid in OPNSense and apply a blacklist to control and monitor network access.

**Walkthrough:**
- **Transparent Proxy Setup:**
  - Configure OPNSense to intercept HTTP/HTTPS traffic transparently.
  - Enable port forwarding and redirect user traffic through Squid for inspection.

- **Web Filtering:**
  - Upload a custom blacklist to OPNSense to restrict access to certain websites, like social media platforms.
  
**Result:** Any user attempting to access restricted sites will receive a filtered page indicating blocked content.

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
