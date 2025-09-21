<img width="1169" height="560" alt="Screenshot 2025-09-21 090115" src="https://github.com/user-attachments/assets/1254e240-c53d-408f-981e-f86884094457" />

---

### CONTENTS

1. Project requirement
2. Introduction about the tools used
5. Test Network/Website
6. Perform Wireshark IDS/Firewall evade techniques
7. Perform NSE Scanning for testing Network/Website
8. Conclusion
9. Reference

---

## ⚠️ Disclaimer

This project is for **educational purposes only**. Unauthorized scanning or exploitation of systems without explicit permission is illegal and unethical. Always obtain proper authorization before conducting any security assessments.

---

## 📦 Requirements

- Kali Linux VM
- Windows VM (optional, for testing)
- Wireshark installed
- Nmap with NSE scripts

---

## 🛠️ Tools Used

- **Wireshark**: Network protocol analyzer for deep packet inspection.
- **Nmap**: Network discovery and security auditing tool.
- **NSE (Nmap Scripting Engine)**: Extends Nmap with scripts for automated vulnerability detection.

### Wireshark

Wireshark is a graphical network protocol analyzer that lets us take a deep dive into the individual packets moving around the network. Wireshark can be used to capture Ethernet, wireless, Bluetooth, and many other kinds of traffic.

Wireshark is an open source software project, and is released under the GNU General Public License (GPL). You can freely use Wireshark on any number of computer you like, without worrying about license keys or fees or such.

For download and latest updates: [www.wireshark.org](https://www.wireshark.org)

### Nmap

Nmap ("Network Mapper") is a free and open source utility for network discovery and security auditing. Nmap runs on all major computer operating systems, and official binary packages are available for Linux, Windows, and Mac OS X.

Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics.

The Nmap Scripting Engine (NSE) is one of Nmap's most powerful and flexible features. It allows users to write (and share) simple scripts (using the **Lua programming language**) to automate a wide variety of networking tasks.

For download and latest updates: [https://nmap.org/](https://nmap.org/)

---

## 🧪 Test Websites

The following test sites were used for learning purposes:

- [https://www.hackthissite.org](https://www.hackthissite.org)
- [http://testphp.vulnweb.com](http://testphp.vulnweb.com)
- [http://testfire.net/index.jsp](http://testfire.net/index.jsp)

---

## PERFORM WIRESHARK IDS/FIREWALL EVADE TECHNIQUES

An **intrusion detection system (IDS)** is a network security tool that monitors network traffic and devices for known malicious activity, suspicious activity or security policy violations.

An IDS cannot stop security threats on its own. Today IDS capabilities are typically integrated with or incorporated into intrusion prevention systems (IPSs), which can detect security threats and automatically act to prevent them.

Being Wireshark a graphical network protocol analyzer that lets us take a deep dive into the individual packets moving around the network. However, it can also be used to simulate and analyze IDS/firewall evasion techniques. Below, I’ll describe **three IDS/firewall evasion techniques**: **Packet Fragmentation**, **Source Port Manipulation** and **IP Address Decoy**, and how we can use Wireshark to analyze or simulate them.

### 🔍 Techniques Demonstrated

#### - **Packet Fragmentation**:

In this technique, the IP packets are split into smaller fragments. By doing this, the TCP header is split across multiple fragments. When IDS encounter the packets, they enqueue them for checking any malicious activity. However, as the number of fragments increases, there is an increase in the CPU and network bandwidth consumption. For this reason, IDS ignores evaluating such packets. Hence, these fragments may pass undetected through the IDS.

The **-f** flag is used to create fragments. So the packet after the IP header is broken down into fragments of size 8 bytes or less, which can help evade IDS and firewalls that analyze larger, more predictable packets.

Some security systems may have difficulty reassembling fragmented packets correctly, allowing the scanner to go unnoticed.

<img width="1062" height="522" alt="image" src="https://github.com/user-attachments/assets/f6aaee6d-68fb-4c20-b138-7270bda6536d" />
Image 1 - Nmap scan screenshot with packet fragmentation

<br><br>
<img width="1383" height="688" alt="image" src="https://github.com/user-attachments/assets/57765d5a-1352-48ba-994d-ccc8673c78be" />
Image 2 - Wireshark screenshot of fragmented and reassembled packets

<br>**Image 2** shows fragmented and reassembled IPv4 packets, with details such as source and destination IP addresses, protocol (TCP), and fragmentation IDs. The image also shows TCP packets with SYN and RST flags.<br>

<br><br>
<img width="1412" height="688" alt="image" src="https://github.com/user-attachments/assets/7a256f25-3db0-4d0d-bcaf-a6677c8c64a0" />
Image 3 - Wireshark screenshot of details of a fragmented IPv4 packet 

<br>**Image 3** shows frame 4033 sent to 192.168.21.220. The highlighted part shows that this frame was reassembled in IP frame 4034. Hence, we can conclude that frame 4033 was a part of frame 4034, and fragmentation was done successfully.<br>

### - **Source Port Manipulation**:

Source Port Manipulation is a technique used by attackers to exploit vulnerabilities in network security systems, particularly firewalls and intrusion detection systems (IDS). It involves manipulating the source port number in the TCP/IP header of a packet to bypass security measures.

When network packets arrive at specific ports, such as port 80 (commonly used for HTTP), IDS may allow them to pass through without rigorous analysis. Attackers can exploit this misconfiguration by altering the source port of packets, causing IDS to ignore malicious traffic.

Nmap uses the **–g** or **–source-port** options to perform source port manipulation to take advantage of these vulnerability.

<img width="1202" height="503" alt="image" src="https://github.com/user-attachments/assets/dde40249-7517-4779-88d2-5671a853e887" />
Image 4 – Nmap scan result with specified source port 

<br><br>
<img width="1380" height="759" alt="image" src="https://github.com/user-attachments/assets/2d858d53-e88c-4f17-8daf-01eb6b113a63" />
Image 5 – Wireshark screenshot with TCP packet with RST and ACK flags 

<br>**Image 5** shows a TCP packet captured in Wireshark, with details such as source (192.168.21.30) and destination (192.168.21.220) IP addresses, ports to 80, and TCP flags (RST and ACK). The packet indicates an attempt to reset the connection, with information such as sequence number (Seq=1), acknowledgement number (Ack=1), and window size (Win=0).<br>

<br><br>
<img width="1442" height="759" alt="image" src="https://github.com/user-attachments/assets/48cc4e04-d301-48c1-8c9c-f34a4e868b89" />
Image  6 - Details of a TCP Packet with RST and ACK Flags in Wireshark   

<br>**Image 6** shows frame 245 sent to the destination 192.168.21.220. The highlighted section indicates that this frame contains a TCP packet with the "RST" flag set (0x004). This signifies that the connection was reset by the sender. The sequence and acknowledgment numbers are both set to 1, and the TCP segment length is 0, confirming that no data was transmitted in this packet.<br>

### - **IP Address Decoy**:

Refers to generating or manually specifying IP addresses of the decoys to evade IDS/firewalls. It appears to the target that the decoys as well as the host(s) are scanning the network. This technique makes it difficult for the IDS/firewall to determine which IP address is actually scanning the network and which IP addresses are decoys.

Nmap has two options for decoy scanning:

- Nmap -D RND:10 [target]
- Nmap –D decoy1, decoy2, decoy3,... [target]

<img width="1297" height="480" alt="image" src="https://github.com/user-attachments/assets/d52945c7-f086-4c96-9a08-ba0a6601a497" />
Image 7 – Wireshark screenshot with TCP packet with RST and ACK flags 
<br>

<br>
The -D flag can be used for a decoy scan. RND option is used for generating random IP addresses. The hosts you will be using as decoys must be online for this method to work.
<br>

<br><br>
<img width="2816" height="632" alt="image" src="https://github.com/user-attachments/assets/5e13d032-9e0c-4895-832d-ed50596d6110" />
Image 8- Nmap scan screenshot with Decoy on testphp.vulnweb.com 
<br>

<br>
We can see that packets were sent to the same target host from 10 different IP addresses of the attacker as is shown in the image above.
<br>

---

## PERFORM NSE SCANNING FOR TESTING NETWORK/WEBSITE

The Nmap Scripting Engine (NSE) is one of Nmap's most powerful and flexible features. It allows users to write (and share) simple scripts to automate a wide variety of networking tasks. Those scripts are then executed in parallel with the speed and efficiency you expect from Nmap.

NSE is activated with the -sC option (or --script if you wish to specify a custom set of scripts) and results are integrated into Nmap normal and XML output.

Example:

- nmap -sC -p22,111,139 -T4 localhost

<br>

  **-sC**: Runs default NSE (Nmap Scripting Engine) scripts.

  **-p22,111,139**: Scans specific ports (22, 111, and 139).

  **-T4**: Sets the timing template to aggressive (faster scan).

  **localhost**: The target of the scan, which is the local machine.
<br>

<br>
NSE scripts are organized into 14 categories on the NSE Scripts documentation page. Many categories are security-oriented, while others hint at discovery and troubleshooting.
<br>


<br>Some of the more interesting categories are:<br>


<br>

- **broadcast**

- **default**

- **discovery**

- **intrusive**

- **vuln**

<br>

There are 604 scripts available on the NSE Scripts page.

I am going present and explain three NSE Script, and I will perform the windows system and testing website susch as testphp.vulnweb.com. And I will explain the script results. 

### **1. --script vuln*


<img width="1405" height="742" alt="image" src="https://github.com/user-attachments/assets/383a6322-a238-4a3e-84ed-13142404cad5" />
<img width="1405" height="742" alt="image" src="https://github.com/user-attachments/assets/a5487772-f937-4d52-9c93-c0fc5f4e5394" />
Image 9 – NSE –-script vuln scan<br>

<br>
The attached images above show the output of an Nmap scan performed with the –-script vuln option target in the host testphp.vulnweb.com (IP: 44.228.249.3) detecting ports 21 (FTP) and 80 (HTTP) open. The http-cross-domain-policy vulnerability was identified, indicating that the crossdomain.xml file has overly permissive configurations, which could allow attacks such as Cross-Site Request Forgery (CSRF) and unauthorized access to sensitive data.

<br>

### **2. --script default*

<img width="2181" height="766" alt="image" src="https://github.com/user-attachments/assets/71a5d4f3-97cc-42f5-a988-8de86fc84ecb" />
Image 10 – NSE –-script default scan<br>

<br>

The **image 11** show the output of an Nmap scan performed with the **–-script default** option target in the host **192.168.21.220** identifying the following open ports: 22 (SSH), 135 (MSRPC), 139 (NetBIOS-SSN), 445 (Microsoft-DS), and 5357 (WSDAPI). The host has a Oracle VirtualBox network interface (MAC: 08:00:27:E6:DD:EA). Script results indicate that NetBIOS is active with the name **WIN-FPGIFTKT6TQ**, and SMB has message signing enabled but not required. The system date and time are 2025-03-20T20:17:10.

<br>

### **3. ----script http-sql-injection*


<img width="2450" height="803" alt="image" src="https://github.com/user-attachments/assets/d8620980-e591-423f-984f-cd6cc5d3988c" />
Image 11 – NSE –-script http-sql-injection scan<br>


<br>

The attached images above show the output of an Nmap scan performed with the **–-script http-sql-injection** option target in the host **testphp.vulnweb.com (IP: 44.228.249.3)** identifying the following open ports: 21 (FTP), 80 (HTTP), 554 (RTSP), and 1723 (PPTP). The analysis using the **sql-injection script** revealed possible SQL Injection vulnerabilities in several URLs of the website, particularly in search pages, product listings, and image display pages.

---

## CONCLUSION

This presentation explored in detail network scanning techniques using Wireshark and Nmap with NSE (Nmap Scripting Engine), focusing on identifying vulnerabilities in networks and test sites like testphp.vulnweb.com to prevent unauthorized exploitation by attackers.

Demonstrated the use of NSE scripts, such as --script vuln, --script default, and --script http-sql-injection, highlighting specific vulnerabilities in target systems.

This presentation reinforces the importance of proactive network security measures and the need for continuous monitoring and vulnerability assessment. The knowledge shared in this presentation can be applied to real-world scenarios to enhance an organization's security posture and prevent unauthorized access to systems and sensitive data.

---

## REFERENCE

- Chauhan, P. D. (2025). Ethical Hacking: Scanning Day 5 and 6. Oeson Learning Cyber Security Program.

- Garn, D. (2022). 5 scripts for getting started with the Nmap Scripting Engine.

- intrusion-detection-system. (2013, 04 13). Retrieved 03 18, 2025, from ibm.com: https://www.ibm.com/think/topics/intrusion-detection-system

- Lyon, G. (2009). Nmap Network Scanning. nmap Project; 12.2.2008 edition. Retrieved from nmap.org.

- Port Scanning and Recon with nmap, Part 2: The nmap scripts (nse). (2022, 12 30). (Otw) Retrieved 03 19, 2025, from hackers-arise.com: https://www.hackers-arise.com/post/port-scanning-and-recon-with-nmap-part-2-the-nmap-scripts-nse

- Zdrnja, B. (2020, 05 07). Scanning with nmap?s NSE scripts. Retrieved 03 20, 2025, from isc.sans.edu: https://isc.sans.edu/diary/26096






















