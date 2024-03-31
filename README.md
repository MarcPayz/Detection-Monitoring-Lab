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
## Lab Logial Diagram:
![AD logical Diagram](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/a5d13f19-f2b2-4ae6-af1d-4ec88d369c2f)

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

<br>
<br>
<br>

Ref 4 & 5: Editing the file:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/3eaf4063-697a-49e2-8afd-8777295c4e52" alt="Before" style="width: 45%; display: inline-block;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/42d91a6e-f7af-4b09-9f8c-358f378d66f6" alt="After" style="width: 45%; display: inline-block;">
<br>
After executing the previous command, the reference on the left displays the state before any changes were made, while the one on the right reflects the file after editing it using Nano. <br> <br>
To clarify the modifications I made: <br>
I disabled DHCP by entering 'no,' as we intend for this server to have the static IP address 192.168.10.10/24. <br>
Under 'name servers' for DNS, I configured it to use Google's DNS address, which is 8.8.8.8. <br>
For 'routes,' which pertains to our router, I added a default route via 192.168.10.1 for all packets. <br>

<br>
<br>
<br>

Ref 6: Checking if changes were made and connectivity:
![Screenshot 2024-03-23 215035](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/f8efed8b-fbd6-4ec1-9306-b0f03e0eacfd)
<br>
After saving the file edited via Nano, I ran the command 'ip a' to check if the static IP configuration was saved. As indicated by the circled area, it was indeed saved. Next, I pinged google.com to test the machine's internet connectivity, and as shown, three packets were received with 0% packet loss.

<br>
<br>
<br>

Ref 7: Getting Splunk and adding it to share:
![Shares 1](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/7cc714c6-9304-46fc-9e6c-d26c24fd5dac)
<br>
Now its time to add Splunk into our ubuntu VM. To begin, I downloaded Splunk from their website onto my host machine and created a separate folder named 'ADproject.' I placed the Splunk download into this folder. Next, I accessed VirtualBox's settings and navigated to the 'Shared Folders' section for the Ubuntu VM. VirtualBox's shared folder feature allows you to share files and directories between the host operating system (the one running VirtualBox) and the guest operating system (the one running inside the virtual machine). For the folder path, I specified the location of the 'ADproject' folder, and for the folder name, I entered 'ADproject' to indicate that I want this specific folder to be shared because it contains Splunk.

<br>
<br>
<br>
Ref 8 & 9: Downloading Splunk:

![Download shares](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/2336833b-fb2e-47e1-83f5-d54b77d8ac09)
<br>
I booted up my Ubuntu VM and navigated to the directory containing the shared files. Then, I ran the command 'ls -la' to view detailed information about the contents of that folder. As shown in the circled green text, Splunk was present and ready to be downloaded. After completing the download, I executed 'ls -la' again to inspect the folders within the application. To access the 'bin' folder, I used the command 'cd bin' to change our directory. We are changing into the binary (bin) folder because those have binary files that splunk can use.

<br><br><br>

Ref 10: Activating splunk: <br>
Once I changed into the bin folder, I ran the command ./splunk and this is the output:
![Screenshot 2024-03-23 222143](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/45e098d8-65c7-4966-a679-942e246bb95b)
<br>
This indicates that the Splunk server is ready for use and is accessible through the web interface at 'http://splunk:8000.' To access Splunk, I will simply navigate to the web interface from another machine and enter the IP address of our Splunk server, which is the static IP confirmed in Reference 6. The port '8000' signifies that Splunk is running on port 8000. To simulate what I will enter in the web interface's URL, it will be '192.168.10.10:8000.

<br>
<br>
<br>

Now switching to the windows 10 target machine. The very first thing I did was rename the pc to target-PC so we would easily identify it on Splunk when we login and monitor. 
<br>
<br>
<br>
Ref 11: Changing Ip address of target-PC:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/d47034be-d5f0-44c6-8b98-0e0720cfe3be" alt="Before" style="width: 45%; display: inline-block;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/24e7a527-85c4-47b2-8b21-4d243e228884" alt="After" style="width: 45%; display: inline-block;">
<br>
To begin with the target-PC, I changed the 192.168.10.5 DHCP address to a static ip address of 192.168.10.100. I gave it a static IP address to just make sure i don't have connectivity issues when communicating to the splunk server. 

<br>
<br>
<br>
Ref 12: Installing Splunk Universal Forwarder:

![Screenshot 2024-03-25 133013](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/e56ae60b-3bdc-4c71-b89b-9404f1d96502)
During the installation of Splunk Universal Forwarder, when configuring it, the IP address specified on the receiving indexer points to the Splunk server. This indicates that all logs will be forwarded to that IP address on port 9997.

<br>
<br>
<br>

To install sysmon, I first had to on the microsoft website to download sysmon, then I had to download a specific configuration file which is called "sysmonconfig.xml" and save it in the sysmon64 folder.
<br>
<br>
Ref 13: Finishing the sysmon process:
![Screenshot 2024-03-25 134147](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/0001d435-8c38-48b9-8332-00bcdfc2f82b)
To start the sysmon service I headed over to powershell, and changed my directory to the sysmon folder. Then I ran the command ".\Sysmon64.exe -i ..\sysmonconfig.xml". To breakdown eveything in that command, ".\Sysmon64.exe" is invoking the Sysmon executable file named "Sysmon64.exe" using the current directory notation. This tells PowerShell to run the executable file located in the current directory. <br> <br> The "-i" instructs Sysmon to install itself on the system according to the configuration provided. <br><br> As for "..\sysmonconfig.xml", the ..\ notation indicates that the configuration file for sysmon is located in the previous directory of the current directory. The file name "sysmonconfig.xml" is the actual configuration file that we want to utilize. 

<br>
<br>
<br>

Ref 14: Configuring Splunk Universal Fowarder:
![Screenshot 2024-03-25 134935](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/c3990a3a-7519-445d-9555-131ccb2f0937)
<br>
Now I need to configure the telemetry I want Splunk Universal Fowarder to send over to the Splunk server. "Index=enpoint" represent what I will be quering on Splunk to get the log I need. The "enpoint" in this case will be the windows 10 machine and the windows server 2022 machine because I will be doing the same configurations on there as well. <br> <br> The "WinEventLog://Application, Security, System, and sysmon" basically means I want to foward all the logs from those categories into Splunk. "Disabled = false" just means I don't want to disable those logs and I want those logs to be sent over. <br><br>
I will be saving this notepad file as "inputs.conf" and save it in Splunk Universal Fowarder directory, specifically under the "local" directory". Any time I make changes to this configurations file, I will need to restart the Splunk Universal Service on windows services. 

<br>
<br>

Ref 15: Local System Account:
![Screenshot 2024-03-25 135316](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/d1d6d53e-e02b-405e-a9d1-9486ece40a82)
The final configuration step for the Splunk Universal Forwarder is determining the service's 'Log on as' setting, crucial for sending telemetry data. Navigating to Windows Services and accessing the service's 'Properties' by right-clicking, I'll select 'Log on as' to be 'Local System account.' This choice grants the Splunk Forwarder access to all local system resources—files, registry keys, and network resources—with full control permissions. This approach ensures comprehensive log collection and transmission to Splunk.
<br> <br> The default option might encounter permission limitations preventing it from collecting logs so we want to avoid that. 

<br>
<br>
<br>

Switching over to Splunk, opened the web browser, and typed '192.168.10.10:8000' into the URL bar. Then, I logged into Splunk using my credentials

<br> 
<br>
Ref 16: Creating a new Index on Splunk:

![new index](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/6beb23b6-3523-4623-bbde-f0435778f33f)
Now that we are logged into Splunk, I need to create a new index. An index is like a folder where data is stored. When you put data into Splunk, it goes into an index. Indexes help organize and manage data, making it easy to search for and analyze later. <br> <br> I am naming this index "endpoint" because that name needs to match what I put in the inputs.conf file on Ref 14. After that I will click on save to lock it in. 

<br>
<br>
<br>

Ref 17: Reciving Port:
![Port](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/f45733bf-512f-44ff-b280-b37da3fb52f2)
The final configuration task on Splunk is to ensure it listens on port 9997 for incoming telemetry from the Splunk Forwarder. As mentioned in Reference 12, the Splunk Forwarder sends logs by default on port 9997. Thus, I only needed to ensure this configuration on Splunk's end; otherwise, no data will be available.

<br>
<br>
<br>
I will replicate the installation and configuration of sysmon and splunk universal fowarder on the windows server 2022. 

<br>
<br>
<br>

Ref 18: Checking Splunk for both hosts and configuration:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/8b3105a2-e8dc-4f12-912a-5a38f894233d" alt="First" style="width: 45%;">
    <img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/eb8b4ac8-ab9a-4ea4-af4d-60d5697084ad" alt="Second" style="width: 45%;">
<br>
The first image shows me searching for 'index=endpoint'. Upon selecting on 'host' the results confirm that I have configured everything correctly because both of my endpoints, 'TARGET-PC' and 'ADPAYZ' (which is a Windows Server 2022), are recognized by Splunk, and it's already displaying various telemetry data gathered about them. <br> <br> The second image displays the 'source,' indicating the origin of the logs and reflecting the configurations set in the inputs.conf file. These configurations specify that logs related to system, application, security, and sysmon will be collected.

<br>
<br>
<br>
Switching over to ADPAYZ where I will show steps reguarding setting up active directory to simulate a real world enterprise environment.
<br>
<br>
<br>
Ref 19: Setting up static ip address:

![Static ip](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/e2a4bc27-bb57-48c0-a2c5-05ec707fe2bb)
To get the Active Directory ball rolling, I need to make sure the ADPAYZ server has the static ip address of 192.168.10.7. This ensures that this vm is in the same network as everything else, and to double check that, I pinged Splunk's ip address of 192.168.10.10 to solidify that they can communicate with each other. 

<br>
<br>
<br>
Ref 20: Installing Domain Services:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/a2349554-5aa2-4480-a915-c40602255060" alt="First" style="display:inline; width:45%;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/060cd81c-6924-482b-ac17-5dbc27209a8f" alt="Second" style="display:inline; width:45%;"> <br>
To begin installing domain services, I will head over to server manager and hover my mouse over manage and select "Add Roles and Features". <br> <br> The second image shows the next step, and upon reaching "Server Roles" I will select "Active Directory Domain Services" and select "Add Features".

<br>
<br>
<br>
Ref 21: Promoting server to Domain Controller:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/3b4e6e81-94fc-4e28-97bd-0005c4f0d9e7" alt="Installation" style="display:inline; width:45%;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/ae0ce4b2-74f7-4719-80be-d4c5c52ee090" alt="Promo" style="display:inline; width:45%;"> <br>
The first image shows the installation for Active Directory Domain Services has succeeded on our server. <br> <br> The second image shows the next step which is heading back to server manager and selecting the option "Promote this server to a domain controller".

<br>
<br>
<br>

Ref 22: Domain Name & Installation:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/ebb03a18-6353-4864-ad79-b92f74a03166" alt="Forest" style="display:inline; width:45%;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/aed41599-74e4-40bd-bb34-032fd763f4fd" alt="Prereq" style="display:inline; width:45%;"> <br>
Next step includes selecting "add a new forest" which means creating a separate and independent instance of Active Directory. In other words this new forest operates on its own, with its own set of rules, users, and resources. I will be naming my root domain name as "payz.local". <br><br> The second image shows the "Prerequisites check" tab where it'll just make sure your machine meets the prerequisites to install Active Directory Domain Services. After that is done, I will select install and restart my machine.

<br>
<br>
<br>

Ref 23: Login:
![Login](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/5f969dfe-e401-4645-b618-1c2f40ad0722)
Logging back into the server, I know everything went well when I see "PAYZ\Adminitrator". "PAYZ" is the domain every new user will join when logging into their account.

<br>
<br>
<br>

Ref 24: Creating OU for users:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/be25aafc-017d-469f-b510-02aebdc08a33" alt="Tools" style="display:inline; width:45%;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/acfd37fd-15c0-4ba6-9c9c-b04291068d3f" alt="OU" style="display:inline; width:45%;"> <br>
To begin the process of adding new users, I will hover over "tools" on server manager and select "Active Directory Users and Computers"
<br> <br> Next I will click on the drop down for "payz.local" and select "New" into "Organizational Unit" (OU).

<br>
<br>
<br>

Ref 25: User Creation Process Dior:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/c027a096-8573-4244-8f67-4a9310665aeb" alt="IT" style="display:inline; width:45%;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/1e4717be-8112-400e-8278-7205aa31a859" alt="DiorP" style="display:inline; width:45%;"> <br>
The first image: After creating the OU called "IT" which will represent users that are working in the Information Technology (IT) department in an organization, I will select "New" and select "User" <br> <br> The second image: The user Dior Payz logon name will be "DiorP" with his own set of credentials to login from another computer on the same domain. 

<br>
<br>
<br>

Ref 26: Result: 
![User](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/73e7f361-ae02-453c-9571-88b81d655547)
This shows a new user (DiorP) was successfully made in the IT OU. 

<br>
<br>
<br>
Ref 27: HR OU:
<br>
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/0b7b451e-9a60-455c-b73b-d9693880e960" alt="Jenny" style="display:inline; width:45%;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/71268136-aab8-47b9-bc96-a35c9ac65a5c" alt="Rresult" style="display:inline; width:45%;"> <br>
I created another OU called "HR" which will represent all the HR employees in the organization and one of the users is "Jenny Smith". She will have the username JennyS with her own set of credentials to login. 

<br>
<br>
<br>
Now I will be adding the target-PC into the domain Payz

<br>
<br>
<br>

Ref 28: Adding target-pc into Domain:
![domain](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/2fbbb6f4-498a-4270-97bc-be41a9350f49)
![Error](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/73dc990c-1b17-4e7a-b065-ae5437431241)
"To add the target PC to the domain, I will navigate to System Properties and select 'Change.' Then, I'll type 'PAYZ.LOCAL' into the 'Member of' field and click OK. However, during this process, I encountered an error indicating that my Active Directory server couldn't be contacted.
<br> <br> I've dealt with this error before, so I instantly know what to do. To fix it, I need to point my DNS configuration toward my Active Directory Domain Controller's IP address.

<br>
<br>
<br>
Ref 29: Changing DNS ip config:

![dns'](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/f021107c-be06-43b9-80c0-3a6c1f7205db)
To change the DNS IP configuration, I will navigate to 'Network Connections' > 'Ethernet Properties' and select 'Internet Protocol Version 4 (IPv4)' and then proceed to its properties. In the 'Preferred DNS server' field, I will change it to the ADPAYZ server's IP address, which is 192.168.10.7. 

<br>
<br>
<br>

Ref 30: Joining the Domain:

<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/9d1f2704-5c50-4fd0-90df-4e71f07069d7" alt="welcome" style="display:inline; width:45%;">
<img src="https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/98869bff-b9ec-4409-8351-9d86349a4902" alt="Jenny" style="display:inline; width:45%;"> <br>
Now that I've pointed the DNS towards ADPAYZ, I can repeat the same steps to successfully join the domain PAYZ.LOCAL. <br><br> The second image shows me authenticating with Jenny's credentials and successfully logging in.

<br>
<br>
<br>

Ref 31: Kali Linux Config: 
![kali1](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/7c717cee-d4e2-4d72-bad4-746d320d92b6)
Switching over to the Kali Linux VM, I need to ensure that it's on the same network as every other VM. To do that, I'll navigate to 'Wired connection 1' and access the IPv4 settings. For the IP address, I will assign it as 192.168.19.250, with a network mask of /24 and the gateway set to 192.168.10.1. As for DNS, I'll use Google's DNS (8.8.8.8).

<br>
<br>
<br>
Ref 32: Checking for connectivity:

![connectivity](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/96613a9f-e4dd-498d-9bbb-8ff90e1a261a)
On the command line, I typed 'ip a' to check if my configurations were set, and as you can see, it was successfully changed to 192.168.10.250/24. To double-check that the Kali Linux VM was on the same network as everything else, I pinged Splunk's IP address, which is 192.168.10.10, and received all packets with zero packet loss.

<br>
<br>
<br>

This concludes the creation of the homelab, now I will act as an adversary by switching to the Kali Linux vm and simmulating possible attacks that can happen in a real world environment. 

<br>
<br>
<br>

Posing as an advesary, I will be utilizing a tool called Crowbar. Crowbar is a tool used in cybersecurity for brute-force attacks. Essentially, it's a program that repeatedly tries different combinations of usernames and passwords until it finds the correct ones to access a system or an account. It's often used by hackers to gain unauthorized access to systems or accounts by guessing login credentials.

<br>
<br>
<br>
Ref 33: Crowbar attack:

![Crowbar](https://github.com/MarcPayz/Detection-Monitoring-Lab/assets/163923336/31388c72-ecfd-423e-8ac4-ac5d32de131b)
As an advasary, I utilied the command "crowbar -b rdp -u DiorP -C passwords.txt -s 192.168.10.100/32". To explain this crowbar command into detail: <br> <br>
-b rdp: This option specifies the service or protocol to target, in this case, RDP (Remote Desktop Protocol). Crowbar will attempt to perform a brute-force attack on RDP services enabled on that machine. <br> <br>

-u DiorP: This option specifies the username to use during the brute-force attack. In this case, the username is "DiorP". <br><br>

-C passwords.txt: This option specifies the path to a file containing a list of passwords to use during the brute-force attack. Crowbar will try each password in this file in an attempt to gain access to the RDP service. <br> <br>

-s 192.168.10.100/32: This option specifies the target IP address or IP range to attack. In this case, crowbar will attempt to brute force the specific ip address of 192.168.10.100. <br><br>

After a while, crowbar results in a RDP-SUCCESS meaning the RDP service was successfully bruteforced and it also provides the password used for that service which is "Mypeopleisgood1" which isn't the most secure password.
































































 

