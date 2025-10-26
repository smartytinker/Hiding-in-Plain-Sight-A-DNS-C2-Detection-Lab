<b>Hiding in Plain Sight: A DNS C2 & Detection Lab ("Tunnelslot") </b>

This project details the construction, execution, and detection of a covert Command & Control (C2) channel operating over the Domain Name System (DNS) protocol within an isolated virtual lab environment. It demonstrates the "Tunnelslot" concept, combining "Living off the Land" (LotL) techniques with DNS tunneling.

⚠️ Disclaimer

This project is strictly for educational and research purposes only. The techniques demonstrated should only be performed in isolated, controlled environments on systems you own or have explicit permission to test. Unauthorized use of these techniques on networks or systems is illegal and unethical.

Concept: "Living off Tunnelslots"

This project explores a methodology focused on combining two stealthy attack techniques:

"Living off the Land" (LotL): Utilizing legitimate, pre-installed system tools (specifically Windows PowerShell in this lab) to execute malicious actions, thereby bypassing traditional signature-based antivirus and blending in with normal administrative activity. 

DNS Tunneling (The "Tunnelslot"): Abusing the DNS protocol, which is almost universally allowed through firewalls, as a covert channel ("tunnelslot") to exfiltrate data and maintain C2 communications. Data is encoded within DNS queries (often in subdomains) directed to an attacker-controlled server. 🕳️

Lab Overview 🔬

A simple, isolated virtual lab was used for this demonstration:

<ul>
<li><b>Attacker:</b> Kali Linux VM running the <code>dnscat2</code> server.</li>
<li><b>Victim:</b> Windows 10/11 VM running the <code>dnscat2</code> PowerShell client.</li>
<li><b>Network:</b> Host-only virtual network with the victim's DNS explicitly set to the attacker's IP.</li>
<li><b>Analysis Tool:</b> Wireshark on the victim VM for capturing DNS traffic.</li>
</ul>

Attack Phases Summary ⚔️

The attack simulation involved:

<ol>
<li><b>Server Setup:</b> Starting the <code>dnscat2</code> server on Kali.</li>
<li><b>Client Execution:</b> Running the <code>dnscat2.ps1</code> script via PowerShell on the Windows victim.</li>
<li><b>C2 Establishment:</b> Verifying the successful connection back to the server.</li>
<li><b>Remote Command Execution:</b> Obtaining a remote shell (<code>cmd.exe</code>) and executing commands like <code>ipconfig</code>.</li>
<li><b>Data Exfiltration:</b> Using the <code>dnscat2</code> <code>download</code> command to steal a file (<code>Secret.txt</code>) over the DNS tunnel.</li>
</ol>

Detection Summary (Blue Team) 

Despite its stealth, the attack generates detectable artifacts:

<ul>
<li><b>Network (Wireshark):</b>
<ul>
<li><b>Anomalous DNS Queries:</b> High volume of DNS requests (<code>TXT</code>, <code>CNAME</code>, <code>MX</code>) to a single, non-standard domain (<code>evil2.com</code>).</li>
<li><b>High Entropy Subdomains:</b> Queries contain long, hex-encoded strings instead of readable hostnames.</li>
<li><b>Direct Endpoint-to-Server DNS:</b> Traffic goes directly between endpoint and attacker IP, bypassing normal corporate DNS infrastructure.</li>
</ul>
</li>
<li><b>Host (Sysmon):</b>
<ul>
<li><b>Suspicious Process Creation:</b> <code>powershell.exe</code> launching with unusual parameters or initiating network connections.</li>
<li><b>Anomalous Network Connections:</b> <code>powershell.exe</code> making direct outbound connections on UDP port 53.</li>
<li><b>Unusual Process Chains:</b> <code>powershell.exe</code> spawning <code>cmd.exe</code>.</li>
</ul>
</li>
</ul>

Tools Used 🛠️

<ul>
<li><a href="https://github.com/iagox86/dnscat2">dnscat2</a> (Server & PowerShell Client)</li>
<li><a href="https://www.vmware.com/">VMware</li>
<li>Kali Linux</li>
<li>Windows 10/11 Evaluation</li>
<li><a href="https://www.wireshark.org/">Wireshark</a></li>
</ul>

Full Report 📄

For a detailed walkthrough, analysis, mitigation strategies, and annotated screenshots, please see the full Report.pdf included in this repository.
