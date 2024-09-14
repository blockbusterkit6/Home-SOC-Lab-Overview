# Home SOC Lab Overview
Objective
The objective of this project was to create a virtual SOC (Security Operations Center) lab environment to simulate a small business network. This included setting up virtual machines, deploying a firewall, implementing a cloud-based SIEM solution, and practicing network traffic analysis and monitoring.

# Process Overview
### 1. Planning and Prerequisites
I began by assessing my hardware resources to ensure my PC had sufficient capacity (16 GB RAM, 50 GB disk space) to run multiple virtual machines. I also decided to use a Linode cloud server for the SIEM tool (Wazuh) to offload some of the processing from my local machine.

### 2. Virtual Environment Setup
I installed VirtualBox to manage my virtual machines (VMs). During this phase, I created VMs for various roles within the SOC environment:

Windows VM: I installed a Windows 10 trial version to simulate an endpoint. Network configuration was a bit challenging as I had to ensure this VM could securely communicate with the Wazuh server on the Linode cloud while remaining isolated from my physical network. I resolved this by setting up a NAT network within VirtualBox.

Linux VM (Kali): I set up a Kali Linux VM to use as an attacker box for simulating network attacks. During the installation, I encountered a problem where the Kali VM was unable to capture traffic from other VMs on the network, which was crucial for my planned penetration tests. After some troubleshooting, I discovered that the issue was due to the VM's network adapter not being set to "Promiscuous Mode." I resolved this by changing the network adapter settings in VirtualBox to enable "Promiscuous Mode: Allow All," which then allowed the Kali VM to capture and analyze network traffic from the other VMs.

### 3. Linode Cloud Server for Wazuh
To centralize monitoring, I decided to deploy Wazuh on a Linode cloud server instead of using Security Onion locally. Here’s how I accomplished this:

Server Setup: I created a Linode cloud server and installed Ubuntu as the base operating system.
Wazuh Installation: I installed Wazuh by following the official documentation. This process involved:
Setting up Elasticsearch and Kibana for log storage and visualization.
Installing the Wazuh server and agent components.
Connectivity: Configuring the Wazuh agent on the Windows and Linux VMs was a challenge. I had to ensure proper network routing so the VMs could send logs to the Wazuh server on the cloud while maintaining security. I configured firewall rules on both the VMs and the Linode server to allow secure log transmission.

### 4. Network Configuration
Within VirtualBox, I set up a NAT network that allowed all the local VMs to communicate. On the Linode side, I configured Wazuh to accept incoming connections from my VM network's IP range. The main challenge here was ensuring secure and stable connectivity between the local VMs and the cloud-based Wazuh server, which required adjustments to network adapter settings and firewall configurations.

### 5. Installing Security Tools
pfSense (Firewall): I created a pfSense VM to act as the network firewall. During the setup, I configured the virtual network interfaces and established basic firewall rules to monitor traffic between VMs. This process required several iterations to fine-tune the firewall rules and ensure proper logging.

Wazuh Integration: I installed the Wazuh agents on both the Windows VM and the Kali Linux VM. The main challenge was correctly registering these agents with the cloud-based Wazuh server. I worked through connection issues by troubleshooting firewall settings and using Wazuh's documentation for guidance.

### 6. Simulating Network Traffic and Attacks
I used the Kali Linux VM to simulate network attacks on the Windows VM. This included running Nmap scans and exploiting known vulnerabilities to test Wazuh’s monitoring capabilities. Initially, not all events were being logged by Wazuh, which I later fixed by adjusting Wazuh's agent configurations and ensuring network communication was stable.

### 7. Testing and Verification
I accessed the Wazuh web interface hosted on the Linode server to verify that the logs and security alerts from my VMs were being captured and analyzed. I cross-referenced Wazuh's detection logs with the simulated attacks from the Kali VM to ensure proper event correlation. Some configuration gaps became apparent during testing, but I addressed these by updating Wazuh's monitoring rules and tuning the alerting mechanisms.

## Challenges Faced
Network Configuration: Ensuring secure communication between the local VMs and the cloud-based Wazuh server was complex, involving multiple firewall rule adjustments and network settings fine-tuning.
Wazuh Agent Setup: Registering the VMs with the Wazuh server required careful attention to network routing and agent configuration files. Initially, I faced connectivity issues, which were resolved by modifying firewall rules and agent settings.
Log Analysis: Adjusting Wazuh’s monitoring rules to accurately capture and alert on simulated attacks took time and required several iterations to align with expected security events.
