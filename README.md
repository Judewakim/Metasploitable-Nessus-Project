# Metasploitable Vulnerability Assessment Project
This project is about setting up a Metasploitable 2 virtual machine and a Kali Linux virtual machine in VirtualBox. Then doing some basic hacks on the Metasploitable VM. Then doing a Nessus scan to create a vulnerability report.
<br><br>
Scenario: a client asks me to do a vulnerability assessment on their network. I will first map the network, identify all devices on the network, identify open ports on each device and services/applications running on each port, then do vulnerability scans on each device in the network, then create a report to return to the client including a diagram of their network, a list of vulnerabilities on the devices in their network, and a path to fixing all the vulnerabilities.  
<br>
This idea for this project came from MyDFIR.<br>
https://youtu.be/G7e5Uw6RO_E?si=z9fg1MsDhFgtP9Z_&t=726
<br><br>
_________________________
### [Installations](https://github.com/Judewakim/Metasploitable-Nessus-Project/blob/main/installations.md)<br>

Install VirtualBox on your computer.<br>
Create a Metasploitable virtual machine and a Kali Linux virtual machine.<br>
Install Tenable Nessus Essentials on your computer.<br>
_________________________
### [Attacks](https://github.com/Judewakim/Metasploitable-Nessus-Project/blob/main/attacks.md)<br>

Exploit an argument injection vulnerability.<br>
Gather a list of user accounts from an SMTP server.<br>
Find an account login password on a VNC server.<br>
Log into the root user account of a MySQL server.<br>
Set up a reverse shell.<br>
_________________________
### [Scans](https://github.com/Judewakim/Metasploitable-Nessus-Project/blob/main/scans.md)<br>

Do Nessus basic network scans and host discovery scans.<br>
Do nmap scans.<br>
<br>
Follow up with OpenVAS for more network vulnerability scanning and OWASP ZAP and Nikto scans for web application vulnerability scanning.
