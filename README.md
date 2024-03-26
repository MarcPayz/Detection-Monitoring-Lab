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
To begin this Lab I will need to install various operating systems (OS) and configure many tools such as Splunk's universal fowarder to push the logs to splunk, and configure Splunk itself and sysmon. I installed four Operating systems to conduct this lab. A regu
