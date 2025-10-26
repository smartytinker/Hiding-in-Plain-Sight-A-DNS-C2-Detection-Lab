# Hiding-in-Plain-Sight-A-DNS-C2-Detection-Lab

Hiding in Plain Sight: A DNS C2 & Detection Lab ("Tunnelslot")

This project details the construction, execution, and detection of a covert Command & Control (C2) channel operating over the Domain Name System (DNS) protocol within an isolated virtual lab environment. It demonstrates the "Tunnelslot" concept, combining "Living off the Land" (LotL) techniques with DNS tunneling.

⚠️ Disclaimer

This project is strictly for educational and research purposes only. The techniques demonstrated should only be performed in isolated, controlled environments on systems you own or have explicit permission to test. Unauthorized use of these techniques on networks or systems is illegal and unethical.

Concept: "Living off Tunnelslots"

This project explores a methodology focused on combining two stealthy attack techniques:

"Living off the Land" (LotL): Utilizing legitimate, pre-installed system tools (specifically Windows PowerShell in this lab) to execute malicious actions, thereby bypassing traditional signature-based antivirus and blending in with normal administrative activity.

DNS Tunneling (The "Tunnelslot"): Abusing the DNS protocol, which is almost universally allowed through firewalls, as a covert channel ("tunnelslot") to exfiltrate data and maintain C2 communications. Data is encoded within DNS queries (often in subdomains) directed to an attacker-controlled server.

Lab Overview

A simple, isolated virtual lab was used for this demonstration:

Attacker: Kali Linux VM running the dnscat2 server.

Victim: Windows 10/11 VM running the dnscat2 PowerShell client.

Network: Host-only virtual network with the victim's DNS explicitly set to the attacker's IP.

Analysis Tool: Wireshark on the victim VM for capturing DNS traffic.

(Optional: Replace placeholder_diagram.png with a simple diagram image file in your img folder showing Attacker VM -> DNS -> Victim VM)

Attack Phases Summary

The attack simulation involved:

Server Setup: Starting the dnscat2 server on Kali.

Client Execution: Running the dnscat2.ps1 script via PowerShell on the Windows victim.

C2 Establishment: Verifying the successful connection back to the server.

Remote Command Execution: Obtaining a remote shell (cmd.exe) and executing commands like ipconfig.

Data Exfiltration: Using the dnscat2 download command to steal a file (Secret.txt) over the DNS tunnel.

Detection Summary (Blue Team)

Despite its stealth, the attack generates detectable artifacts:

Network (Wireshark):

Anomalous DNS Queries: High volume of DNS requests (TXT, CNAME, MX) to a single, non-standard domain (evil2.com).

High Entropy Subdomains: Queries contain long, hex-encoded strings instead of readable hostnames.

Direct Endpoint-to-Server DNS: Traffic goes directly between endpoint and attacker IP, bypassing normal corporate DNS infrastructure.

Host (Sysmon):

Suspicious Process Creation: powershell.exe launching with unusual parameters or initiating network connections.

Anomalous Network Connections: powershell.exe making direct outbound connections on UDP port 53.

Unusual Process Chains: powershell.exe spawning cmd.exe.

Tools Used

dnscat2 (Server & PowerShell Client)

VirtualBox (or VMware)

Kali Linux

Windows 10/11 Evaluation

Wireshark

Sysmon (for host-based detection concepts)

Full Report

For a detailed walkthrough, analysis, mitigation strategies, and annotated screenshots, please see the full Report.pdf included in this repository.

License

This project is licensed under the MIT License - see the LICENSE file for details (Optional: Create a LICENSE file if you want).
