# CyberSecurity Detection & Monitoring Lab

## Objective

The primary objective of this lab is to enhance incident response capabilities by utilizing Splunk for threat detection, while actively monitoring telemetry generated from diverse endpoints and supplementary security tools, including Snort, across the virtualized home lab network.

### Skills Learned

- Experienced in configuring and administering Active Directory environments.
- Familiarity with Splunk SIEM (Security Information and Event Management) platform.
- Log configuration, monitoring and analysis.
- Incident response by developing and executing a plan to respond to simulated attacks occuring on the network.
- NIDS/NIPS Configuration.
  
### Tools Used

- Splunk (SIEM)
- Splunk Universal Fowarder
- Sysmon
- Snort (IPS/IDS)
- Atomic Red Team
  
## General Knowledge
Before we begin I will give a litle background on Splunk, Splunk Universal Fowarder, Sysmon, and Atomic Red Team.
<br>
<br>
Splunk is a leading SIEM (Security Information and Event Management) platform that helps organizations monitor, analyze, and respond to security events and incidents in real-time. It's commonly referred to as a "Data-to-Everything" platform because it enables organizations to gain insights from a wide range of data sources, including logs, events, metrics, sensors, and more.
<br>
<br>
Splunk Universal Forwarder is a lightweight tool that collects data from various sources such as enpoinds and sends it to a Splunk deployment for indexing and analysis.
<br>
<br>
Sysmon, short for System Monitor, is a Windows system service and device driver developed by Microsoft. It provides advanced logging and monitoring capabilities to help detect and investigate suspicious or malicious activity on Windows systems.
<br>
<br>
Atomic Red Team is a framework for simulating both offensive and defensive tactics in cybersecurity. It provides a collection of small, discrete tests called "atomic tests" that simulate specific adversary behaviors or techniques. These tests help organizations evaluate their security controls, detection capabilities, and incident response procedures in a controlled environment.
<br>
<br>
## Lab Logial Diagram
This is the logical diagram of the lab environment. I will utilize four operating systems, including a Windows 10 virtual machine (VM) designated as the target machine. The target VM will have Sysmon and Splunk Universal Forwarder installed to forward various telemetry to Splunk. Additionally, Atomic Red Team will be installed on the target VM to simulate additional adversary attacks.<br> <br> The second operating system will be Windows Server 2022, which will utilize Active Directory Domain Services to organize users into different organizational units (OUs), simulating a real enterprise environment. Windows Server 2022 will also have Sysmon and Splunk Universal Forwarder installed to send telemetry to Splunk in case it becomes a target of adversaries. <br><br> The third operating system will be Ubuntu, which will host our Splunk server running on port 8000. Lastly, Kali Linux OS will serve as the attacker's operating system. <br><br> All vm's will be on the same local network which will be 192.168.10.0/24.
<br>
<br>

## Steps
Ref 1: Creating NAT Network:
![Creating Nat network](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/2df8d314-ff60-4e1f-b3b8-8c1b34b268c7)
After installing all OS's into virtual Box, I will need to make sure all virtual machines are in the same network. To start that, I clicked on tools which led me properties. Then I created a new name which I called it "AD Project" gave it an IPV4 prefix of 192.168.10.0/24. I also enabled DHCP incase for the future I want to add another vm and that can recieve an IP address automatically. 
<br>
<br>
<br>

Ref 2: Adding various VM's into NAT network:
<br>
![Screenshot 2024-03-23 210359](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/0326ba2b-70d7-46f8-9730-9c1a22876a3a)
<br>
The next step involves adding the VMs to my custom NAT network. I did this by accessing the VM settings, clicking on 'Network,' selecting 'NAT network' under Adapter 1, and confirming the selection, which displayed the name 'AD Project.' I repeated this process for the other three VMs.

<br>
<br>
<br>
Ref 3: checking Ubuntu's Ip:

![Ubuntu 1](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/756830d0-66ac-44c8-b501-d82639e87519)
<br>
To begin configuring Splunk's server which is running via ubuntu, I need to give it a static ip of 192.168.10.10/24. To start that process I checked which Ip address it already had by typing "ip a" which is 192.168.10.4/24. Next I typed the command "sudo nano /etc/netplan/00/-installer-config.yaml". <br><br> The command "sudo nano /etc/netplan/00-installer-config.yaml" is used to edit a YAML configuration file named "00-installer-config.yaml" located in the "/etc/netplan" directory. <br><br> To break every thing dowm, "sudo is used in Unix-like operating systems to run programs with the security privileges of another user, typically the superuser (root). <br><br> "Nano" is a text editor that runs in the command line. It's a simple and easy-to-use editor commonly used in Unix-like systems. <br><br>"/etc/netplan" is the directory where netplan YAML configuration files are stored. <br><br>"Netplan" is a utility for configuring networking on Linux distributions, particularly those using systemd. <br><br> "00/-installer-config.yaml" is the specific file that we are going to edit using nano. YAML (YAML Ain't Markup Language) is a human-readable data serialization format commonly used for configuration files in software applications and systems. In this case, the file likely contains network configuration settings for the system.

 

