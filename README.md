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
<b>Setting Up the Azure Environment:</b> This step marks the beginning of the project by provisioning the core Azure infrastructure. A dedicated resource group and virtual network were created, along with two virtual machines, one running Windows and one running Linux. These instances serve as controlled endpoints to simulate real-world communication between different operating systems. Establishing the environment was crucial for testing how Azure handles internal network traffic and applying security rules at scale.
</p>
<br />

<p>
  <h4>2) Infrastructure Validation</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-07-58" src="https://github.com/user-attachments/assets/1981e366-8462-417c-b535-c175a6725823" />
</p>
<p>
<b>Confirming System Deployment:</b> After deployment, both virtual machines were verified to be active and running correctly within the same virtual network. This step ensured that basic connectivity and configuration settings were consistent across both systems. The validation also confirmed that Azure’s dynamic IP assignment placed both VMs within the same subnet, which is essential for testing internal communication. Verifying the infrastructure provided confidence that the environment was stable before security configurations began.
</p>
<br />

<p>
  <h4>3) Default NSG Configuration</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-36-54" src="https://github.com/user-attachments/assets/1e393d83-ce5f-466b-b0e2-8ca77e5c4fac" />
</p>
<p>
<b>Reviewing the Baseline Security Rules:</b> The Network Security Group (NSG) was examined to understand the baseline security posture applied by Azure. These default inbound and outbound rules determine which types of traffic are allowed or denied between connected devices. Observing these initial configurations is vital to avoid conflicts when custom rules are introduced later. This step provided a clear understanding of how Azure isolates traffic and protects virtual machines out of the box.
</p>
<br />

<p>
  <h4>4) Initial ICMP Connectivity</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-28-23" src="https://github.com/user-attachments/assets/57f0abd3-8acf-464a-b5e0-51aac9273445" />
</p>
<p>
 <b>Observing Initial Connectivity:</b>  A ping test was conducted from the Windows VM to the Linux VM to verify connectivity and packet flow. Wireshark captured the ICMP packets in real time, showing successful echo requests and replies. This confirmed that the two systems could communicate freely under the default NSG settings. The results established a clear baseline for measuring how future security rule changes would affect network behavior.
</p>
<br />

<p>
  <h4>5) Verifying Interface Details</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-32-55" src="https://github.com/user-attachments/assets/523eac53-cc03-4d56-b622-b9cb8f95ded1" />
</p>
<p>
<b>Analyzing Network Interfaces:</b> Network adapter details were retrieved using the ipconfig /all command to map MAC addresses and IPs observed in Wireshark. This step helped correlate packet data to specific devices within the virtual network. Understanding these identifiers ensured that each network capture could be traced back to the correct host. It also highlighted how Azure dynamically assigns and manages virtual interfaces for cloud-based machines.
</p>
<br />

<p>
  <h4>6) Configuring NSG Deny Rule</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-40-10" src="https://github.com/user-attachments/assets/83a8c0b4-c605-42ed-8529-532277f404e9" />
</p>
<p>
<b>Implementing a Security Restriction:</b> A new inbound rule was added to the NSG to explicitly deny ICMP (ping) traffic. By setting the action to “Deny” with a high priority, the rule immediately overrode previous allow permissions. This configuration simulated a real-world network lockdown scenario, where administrators restrict certain protocols to harden security. Creating this rule demonstrated the practical control Azure administrators have over inbound traffic.
</p>
<br />

<p>
  <h4>7) Rule Applied Successfully</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-40-33" src="https://github.com/user-attachments/assets/c5f68601-d46b-4af1-8138-60ca04a8914d" />
</p>
<p>
<b>Verifying the Rule Enforcement:</b> The NSG interface confirmed that the new DenyInbound rule was applied successfully and placed above other rules in the priority order. This hierarchy ensures that specific blocks take precedence over broader allowances. The update was visible in Azure’s portal, clearly showing that the ICMP traffic would now be restricted. This provided the perfect setup to observe the impact of rule enforcement on network communication.
</p>
<br />

<p>
  <h4>8) ICMP Block Validation</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-41-47" src="https://github.com/user-attachments/assets/60ad7827-8b6b-4ab4-9263-6a8aee2809d8" />
</p>
<p>
<b>Testing the Effects of the Deny Rule:</b> After applying the deny rule, ping attempts from the Windows VM to the Linux VM began failing. Wireshark showed that echo requests were still sent, but no replies were received, confirming that the NSG rule worked as intended. This test proved how security groups can immediately influence packet flow at the network layer. It also demonstrated how network administrators can quickly verify security configurations through packet inspection tools.
</p>
<br />

<p>
  <h4>9) SSH Traffic Observation</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 10-51-16" src="https://github.com/user-attachments/assets/27796293-3478-4b48-b92d-fb65e0790793" />
</p>
<p>
<b>Capturing Encrypted Communication:</b> A secure SSH session was initiated from the Windows VM to the Linux VM over TCP port 22. Wireshark captured the exchange of encrypted SSH packets, highlighting the confidentiality of this protocol. This part of the lab showed the importance of using secure methods for remote administration. It also illustrated how encrypted traffic appears within packet captures, even though the underlying data remains unreadable.
</p>
<br />

<p>
  <h4>10) DNS Traffic Inspection</h4>
<img width="1920" height="1080" alt="Screenshot from 2025-10-23 11-05-56" src="https://github.com/user-attachments/assets/5e948ec7-ee81-4cf3-aee9-2451d3ac4ec8" />
</p>
<p>
<b>Investigating Name Resolution Traffic:</b> The final phase analyzed DNS queries generated from the Windows VM during a name lookup for “disney.com.” Wireshark recorded both the DNS request and the authoritative response, demonstrating successful resolution through Azure’s internal DNS infrastructure. This test confirmed that name resolution functions correctly. The captured packets provided valuable insight into how DNS operates at the packet level within cloud networks.
</p>
<br />
