<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute/NSG)
- Remote Desktop & SSH
- Various Command-Line Tools
- Various Network Protocols (SSH,DNS,ICMP,RDP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Enterprise (22H2)
- Ubuntu Server 22.04 LTS

<h2>High-Level Steps</h2>

- Deploy Azure Resources (VMs, VNet, NSG)
- Validate Network Connectivity
- Capture Traffic with Wireshark
- Modify NSG Rules to Control Traffic
- Analyze Protocol Behavior (ICMP, SSH, DNS)

<h2>Actions and Observations</h2>

<p>
  <h4>1) Azure Resource Deployment</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 09-57-52" src="https://github.com/user-attachments/assets/467377ea-9446-4546-b8be-6f7073985510" />
</p>
<p>
<b>Setting Up the Azure Environment:</b> Created a new resource group, virtual network, and two virtual machines (Windows & Linux) to simulate network traffic between hosts.
</p>
<br />

<p>
  <h4>2) Infrastructure Validation</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-07-58" src="https://github.com/user-attachments/assets/1981e366-8462-417c-b535-c175a6725823" />
</p>
<p>
<b>Confirming System Deployment:</b> Both virtual machines are successfully deployed and running in Azure. Each resides on the same virtual network, enabling direct communication.
</p>
<br />

<p>
  <h4>3) Default NSG Configuration</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-36-54" src="https://github.com/user-attachments/assets/1e393d83-ce5f-466b-b0e2-8ca77e5c4fac" />
</p>
<p>
<b>Reviewing the Baseline Security Rules:</b> Initial inbound/outbound rules allow ICMP, RDP, and SSH traffic by default. This forms the baseline for testing.
</p>
<br />

<p>
  <h4>4) Initial ICMP Connectivity</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-28-23" src="https://github.com/user-attachments/assets/57f0abd3-8acf-464a-b5e0-51aac9273445" />
</p>
<p>
 <b>Observing Initial Connectivity:</b>  Pinging from Windows VM (10.0.0.4) to Linux VM (10.0.0.5). Wireshark confirms successful ICMP echo requests and replies.
</p>
<br />

<p>
  <h4>5) Verifying Interface Details</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-32-55" src="https://github.com/user-attachments/assets/523eac53-cc03-4d56-b622-b9cb8f95ded1" />
</p>
<p>
<b>Analyzing Network Interfaces:</b> Displaying adapter information (ipconfig /all) to correlate MAC and IP addresses seen in packet captures.
</p>
<br />

<p>
  <h4>6) Configuring NSG Deny Rule</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-40-10" src="https://github.com/user-attachments/assets/83a8c0b4-c605-42ed-8529-532277f404e9" />
</p>
<p>
<b>Implementing a Security Restriction:</b> Creating a new inbound rule to block ICMP (ping) traffic by setting protocol = ICMP, action = Deny, and priority = 290.
</p>
<br />

<p>
  <h4>7) Rule Applied Successfully</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-40-33" src="https://github.com/user-attachments/assets/c5f68601-d46b-4af1-8138-60ca04a8914d" />
</p>
<p>
<b>Verifying the Rule Enforcement:</b> Updated NSG now lists the new “DenyInbound” ICMP rule. This takes precedence over lower-priority allow rules.
</p>
<br />

<p>
  <h4>8) ICMP Block Validation</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-41-47" src="https://github.com/user-attachments/assets/60ad7827-8b6b-4ab4-9263-6a8aee2809d8" />
</p>
<p>
<b>Testing the Effects of the Deny Rule:</b> After applying the rule, ping requests begin timing out. Wireshark confirms the absence of echo replies — verifying NSG enforcement.
</p>
<br />

<p>
  <h4>9) SSH Traffic Observation</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-51-16" src="https://github.com/user-attachments/assets/27796293-3478-4b48-b92d-fb65e0790793" />
</p>
<p>
<b>Capturing Encrypted Communication:</b> Capturing encrypted SSH traffic over TCP port 22 between Windows and Linux VMs, showing secure remote connection behavior.
</p>
<br />

<p>
  <h4>10) DNS Traffic Inspection</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 11-05-56" src="https://github.com/user-attachments/assets/5e948ec7-ee81-4cf3-aee9-2451d3ac4ec8" />
</p>
<p>
<b>Investigating Name Resolution Traffic:</b> Capturing DNS query and response packets during nslookup commands, illustrating name resolution between internal and external hosts.
</p>
<br />
