#  **Nmap & Scapy Lab Documentation**

## **1. Introduction**

This lab focused on practicing **network scanning**, **service enumeration**, and **packet sniffing** using **Nmap** and **Scapy**.
We worked in a controlled internal network and used the target: **10.6.6.23**.

These exercises are foundational for ethical hacking, reconnaissance, and understanding how attackers map and analyze networks.

---

## **2. Lab Environment**

* **Machine:** Linux / Kali Linux
* **Tools Used:**

  * Nmap
  * Scapy
  * Wireshark
* **Target:** `10.6.6.23`
* **Network:** `10.6.6.0/24`

---

#  **3. Nmap Practical Work**

## **3.1 Host Discovery (Ping Sweep)**

**Command:**

```bash
nmap -sn 10.6.6.0/24
```

**Purpose:**
Check which hosts are alive on the network.

**Result:**
10.6.6.23 responded — host is UP.

---

## **3.2 OS Detection**

**Command:**

```bash
sudo nmap -O 10.6.6.23
```

**Purpose:**
Identify the operating system by analyzing network responses.

**Result:**
Nmap returned an OS guess based on TCP/IP fingerprints.

---

## **3.3 Service & Version Detection for FTP**

**Command:**

```bash
nmap -p21 -sV -A -T4 10.6.6.23
```

**Purpose:**
Enumerate services, detect versions, and gather extra details.

**Result:**
Nmap detected the FTP service and additional information useful for penetration testing.

---

## **3.4 SMB Enumeration (Port 445)**

**Command:**

```bash
nmap --script smb-enum-shares.nse -p445 10.6.6.23
```

**Purpose:**
Identify accessible SMB shares.

**Result:**
Showed available shares and potential misconfigurations.

---

## **3.5 Manual SMB Access Test**

**Command:**

```bash
smbclient //10.6.6.23/print$ -N
```

**Purpose:**
Check if anonymous SMB login is allowed.

**Outcome:**
Anonymous access successful → potential security weakness.
(Type `exit` to quit.)

---

# 🔬 **4. Scapy Practical Work**

> *Scapy was used entirely from the interactive shell — no python files.*

## **4.1 Start Scapy**

```bash
sudo su
scapy
```

---

## **4.2 Sniffing All Traffic**

**Command:**

```python
sniff()
```

**Purpose:**
Capture live packets on the default interface.

**Action:**
Opened a second terminal and ran:

```bash
ping google.com
```

**Result:**
Scapy captured ICMP, DNS, and other packets.

To stop sniffing: `Ctrl + C`.

You can store results:

```python
paro = _
paro.summary()
```

---

## **4.3 Sniffing on a Specific Interface**

**Command:**

```python
sniff(iface="br-internal")
```

**Purpose:**
Capture traffic on the internal bridge network.

Triggered traffic by:

* Opening browser → visiting `10.6.6.23`
* Doing network pings

Saved results:

```python
paro2 = _
paro2.summary()
```

---

## **4.4 Filtering Only ICMP**

**Command:**

```python
sniff(iface="br-internal", filter="icmp", count=5)
```

**Test traffic:**

```bash
ping 10.6.6.23
```

Captured exactly 5 ICMP packets.

Stored and inspected:

```python
paro3 = _
paro3.summary()
paro3[3]
```

---

#  **5. Key Learnings**

* Nmap can detect live hosts, services, versions, and OS fingerprints.
* SMB enumeration revealed possible anonymous access weaknesses.
* Scapy allows live packet sniffing and analyzing ICMP, DNS, ARP, and more.
* Using filters makes packet capture more targeted and efficient.
* Hands-on testing shows how attackers gather information early in a penetration test.

---

# 🏁 **6. Conclusion**

This lab strengthened my practical understanding of reconnaissance using Nmap and packet analysis using Sca
