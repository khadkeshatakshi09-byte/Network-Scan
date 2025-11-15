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

