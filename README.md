Network Reconnaissance Using Nmap  
Cyber Security Internship – Task 1

This project involves scanning a local network using **Nmap** to identify active hosts and open ports.  
The purpose is to understand basic **network reconnaissance**, **port scanning**, and **service exposure**.

---

## 📌 **1. My Network Details**
- **Local IP:** 192.168.56.1  
- **Network Range Scanned:** 192.168.56.0/24  
- **System Type:** Windows (based on SMB and RPC ports)  
- **Tool Used:** Nmap GUI (Zenmap)

---

## 🧪 **2. Step 1: Host Discovery Scan (`-sn`)**

### 🔹 Command:
```bash
nmap -sn 192.168.56.0/24
```

### 🔹 Purpose:
- Detect which devices in the network are **ON**
- Does **not** scan ports  
- Equivalent to a "ping sweep"

### 🔹 Screenshot:
![Ping Scan](ping-scan.png)

### 🔹 Result Summary:
- **1 active host found:** 192.168.56.1  
- No other devices responded  
- This means the network currently has only **one live machine**

---

## 🧪 **3. Step 2: TCP SYN Scan (`-sS`)**

### 🔹 Command:
```bash
nmap -sS 192.168.56.1
```

### 🔹 Purpose:
- Perform a **stealthy scan**  
- Identify **open TCP ports** on the device  
- Determine which network services are exposed

### 🔹 Screenshot:
![SYN Scan](syn-scan.png)

### 🔹 Result Summary:
| Port | State | Service |
|------|--------|-----------|
| **135/tcp** | open | msrpc |
| **139/tcp** | open | netbios-ssn |
| **445/tcp** | open | microsoft-ds |

### 🔹 Interpretation:
- **Port 135 – msrpc:** Microsoft RPC service  
- **Port 139 – netbios-ssn:** NetBIOS session for Windows sharing  
- **Port 445 – SMB:** File sharing, authentication  

These ports show the device is running **Windows networking services**.

---

## 📊 **4. What I Learned**
- Running basic Nmap scans  
- Host discovery vs. port scanning  
- Open ports indicate active services  
- SMB, NetBIOS, and RPC are Windows services  
- Importance of securing exposed ports  


## 🧪 4. Wireshark Packet Analysis

To understand how Nmap works at the packet level, I captured the network traffic using **Wireshark** while performing the SYN scan.

### 🔹 Steps I Performed:
1. Opened Wireshark and selected the active network interface  
   (VirtualBox Host-Only Network for 192.168.56.x range).
2. Applied the display filter:
   ```
   tcp.flags.syn == 1
   ```
   This filter shows only TCP SYN packets used in port scanning.
3. Started packet capture.
4. Ran the command:
   ```bash
   nmap -sS 192.168.56.1
   ```
5. Observed the SYN and SYN/ACK responses during the scan.
6. Stopped the capture and analyzed the packet flow.

### 🔹 What I Observed:
- Wireshark captured multiple **SYN packets** sent from my system to the target IP (192.168.56.1).
- For **open ports** (135, 139, 445), I saw:
  - SYN → sent by my machine  
  - SYN/ACK → sent by target  
  - RST → sent by Nmap (SYN scan closes connection without completing handshake)
- This pattern confirms that these ports were **open**.
- No SYN/ACK packets were received from other ports, which means they were **closed**.

### 🔹 What This Shows:
- Nmap uses **half-open scanning (SYN scan)** to detect open ports without establishing a full TCP connection.
- Wireshark visually confirms how port scanning works at the packet level.
- Open Windows ports (135, 139, 445) match the results found in Nmap’s output.

### 🔹 Learning Outcome:
Using Wireshark helped me understand:
- How SYN scans work internally  
- How open and closed ports respond differently  
- How to capture and analyze network traffic  
- The importance of packet-level inspection in cyber security
