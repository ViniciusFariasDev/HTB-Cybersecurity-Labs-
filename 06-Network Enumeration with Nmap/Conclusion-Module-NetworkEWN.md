Conclusion Network Enumeration with Nmap
<img width="886" height="728" alt="image" src="https://github.com/user-attachments/assets/83926bf4-88e2-46da-a630-e2b4bb20ad0d" />

# Network Enumeration with Nmap

**Platform:** Hack The Box Academy  
**Module:** Network Enumeration with Nmap  
**Status:** Completed ✅  
**Learning Path:** Penetration Tester  
**Certification Goal:** HTB Certified Penetration Testing Specialist (CPTS)

---

## 1. Module Overview

Network enumeration is a fundamental phase of penetration testing.

Before investigating vulnerabilities, a penetration tester must understand the target's network exposure, identify accessible services, and analyze how network security controls affect communication.

During this module, I studied Nmap scanning techniques and completed practical exercises involving host discovery, service enumeration, network scanning performance, and firewall evasion.

The exercises reinforced the importance of interpreting network responses rather than relying exclusively on automated scan results.

### Learning Objectives

- Understand the fundamentals of network enumeration.
- Identify active hosts and accessible network services.
- Perform TCP port scanning.
- Identify operating systems and service versions.
- Use the Nmap Scripting Engine (NSE).
- Optimize scanning performance.
- Analyze firewall filtering behavior.
- Understand IDS and IPS technologies.
- Investigate services protected by network security controls.

---

## 2. Host Discovery

Host discovery is the process of identifying active systems within a network.

Nmap provides several methods to determine whether a target is reachable.

### Common Techniques

| Technique | Description |
|---|---|
| ICMP Echo Request | Checks whether a host responds to ICMP packets. |
| ARP Discovery | Identifies active devices on a local network. |
| TCP-Based Discovery | Uses TCP packets to determine host availability. |
| Host Discovery Without Port Scanning | Identifies hosts without performing a conventional port scan. |

### Commands Studied

```bash
sudo nmap -sn <network>
```

Discovers active hosts without performing a conventional port scan.

```bash
sudo nmap -sn -PE <target>
```

Uses ICMP Echo Requests for host discovery.

```bash
sudo nmap -sn --disable-arp-ping <target>
```

Disables ARP-based host discovery.

### Key Takeaway

A host that does not respond to a particular discovery technique is not necessarily offline.

Firewalls and network security controls may prevent certain discovery packets from reaching the target or returning to the scanner.

---

## 3. TCP Port Scanning

Port scanning helps identify network services exposed by a target system.

Nmap classifies ports into different states based on the responses received.

### Port States

| State | Meaning |
|---|---|
| Open | A service is accepting connections. |
| Closed | The port is reachable, but no service is accepting connections. |
| Filtered | Network filtering prevents Nmap from determining the port state. |
| Unfiltered | The port is reachable, but its open or closed state is unknown. |
| Open\|Filtered | Nmap cannot determine whether the port is open or filtered. |
| Closed\|Filtered | Nmap cannot determine whether the port is closed or filtered. |

### TCP SYN Scan

```bash
sudo nmap -sS -p- <target>
```

**Options:**

- `-sS`: Performs a TCP SYN scan.
- `-p-`: Scans all 65,535 TCP ports.

A SYN scan evaluates TCP responses to identify port states without completing the standard TCP three-way handshake for open ports.

### Practical Learning

During the module, I performed full TCP scans to identify services running on standard and non-standard ports.

I also learned that relying exclusively on common port numbers can lead to incorrect assumptions about the services running on a target.

---

## 4. Service and Version Detection

Service enumeration involves identifying the applications and protocols running on accessible network ports.

Nmap provides version detection capabilities through the `-sV` option.

### Example

```bash
sudo nmap -sV -p 22 <target>
```

### Practical Exercise: Operating System Identification

**Target:** `10.129.2.80`

The initial scan identified:

```text
22/tcp   open      ssh
80/tcp   open      http
139/tcp  filtered  netbios-ssn
445/tcp  filtered  microsoft-ds
```

I then investigated the SSH service:

```bash
sudo nmap -sV -p 22 10.129.2.80
```

### Result

```text
22/tcp open ssh OpenSSH 7.6p1 Ubuntu 4ubuntu0.7
Service Info: OS: Linux
```

### Analysis

The SSH banner revealed information associated with Ubuntu Linux.

This demonstrated that operating system identification does not always require aggressive operating system fingerprinting.

In some situations, service banners provide sufficient information to identify the likely operating system or Linux distribution.

### Key Takeaway

Service enumeration can reveal valuable information about the underlying operating system, installed software, and potential attack surface.

---

## 5. Nmap Scripting Engine (NSE)

The Nmap Scripting Engine extends Nmap's functionality through scripts designed for information gathering, service enumeration, and security assessments.

### Common NSE Categories

| Category | Purpose |
|---|---|
| auth | Authentication-related checks |
| default | Commonly used scripts |
| discovery | Network and service discovery |
| safe | Scripts generally designed to avoid disruptive behavior |
| version | Additional service version detection |
| vuln | Checks for known vulnerabilities |

### Commands Studied

```bash
sudo nmap -sC <target>
```

Runs Nmap's default NSE scripts.

```bash
sudo nmap --script banner -p <port> <target>
```

Attempts to retrieve service banners.

```bash
sudo nmap -p 80 -sV --script http-enum <target>
```

Enumerates common HTTP resources.

### Practical Learning

During an enumeration exercise, I investigated a service running on a non-standard port.

The standard service detection results did not initially provide all the information needed.

Using the NSE banner script helped retrieve additional service information.

I also practiced web enumeration and identified a `robots.txt` resource.

### Key Takeaway

NSE scripts can reveal additional information that is not always displayed during a standard port scan.

---

## 6. Scanning Performance

Nmap provides several options to control scanning speed, timeouts, retries, and packet transmission rates.

Understanding these options is important when assessing large networks or systems protected by security monitoring technologies.

### Important Parameters

| Parameter | Description |
|---|---|
| `-T0` to `-T5` | Controls the timing template. |
| `--min-rate` | Specifies the minimum packet transmission rate Nmap attempts to maintain. |
| `--max-retries` | Limits probe retransmissions. |
| `--max-rtt-timeout` | Controls the maximum response timeout. |
| `--initial-rtt-timeout` | Sets the initial response timeout estimate. |

### Example

```bash
sudo nmap -sS -p- --min-rate 1000 -T4 <target>
```

This command performs a full TCP SYN scan using an aggressive timing template and a specified minimum packet transmission rate.

### Key Takeaway

Increasing scanning speed may reduce the reliability of results, particularly when packet loss, high latency, or security filtering is present.

**Scanning accuracy is often more important than scanning speed.**

---

## 7. Firewall, IDS, and IPS

Modern networks use multiple security controls to monitor and restrict communication.

### Firewall

A firewall controls incoming and outgoing traffic according to configured security rules.

Depending on its configuration, it may allow, reject, or silently drop packets.

### Intrusion Detection System (IDS)

An IDS monitors network or host activity and generates alerts when suspicious behavior is detected.

### Intrusion Prevention System (IPS)

An IPS can actively prevent suspicious activity by blocking connections or dropping packets.

### IDS vs. IPS

| Feature | IDS | IPS |
|---|---|---|
| Detects suspicious activity | Yes | Yes |
| Generates alerts | Yes | Yes |
| Can automatically block traffic | Not typically | Yes |
| Primary purpose | Detection | Prevention |

### Key Takeaway

Security monitoring and traffic filtering can influence the results of network enumeration.

Understanding these systems helps penetration testers interpret unexpected responses and investigate potential filtering rules.

---

## 8. Firewall Evasion — Practical Lab

### Lab Objective

Identify a service protected by firewall filtering rules and investigate its version.

**Target:** `10.129.2.47`

**Environment:** Authorized HTB Academy laboratory.

### Step 1 — Initial TCP Enumeration

I started with a full TCP port scan:

```bash
sudo nmap -p- --open 10.129.2.47
```

### Result

```text
22/tcp open ssh
80/tcp open http
```

The scan also reported a significant number of filtered ports.

This suggested that network filtering was affecting visibility into the target's exposed services.

### Step 2 — HTTP Investigation

I inspected the HTTP response headers:

```bash
curl -I http://10.129.2.47
```

### Result

```text
HTTP/1.1 200 OK
Server: Apache/2.4.29 (Ubuntu)
```

The response identified an Apache web server running on Ubuntu.

I also inspected the page content and confirmed that it was the default Apache web page.

This did not reveal the additional service required by the exercise.

### Step 3 — Source-Port-Based Scanning

I investigated whether the firewall treated packets differently depending on their source port.

```bash
sudo nmap -sS -Pn -n -p- --open --source-port 53 10.129.2.47
```

### Result

```text
22/tcp    open ssh
80/tcp    open http
50000/tcp open ibm-db2
```

An additional port became visible.

**Important:** The `ibm-db2` label represented Nmap's default service association for port 50000, not a confirmed identification of the running application.

### Step 4 — Service Version Detection

I attempted to identify the service:

```bash
sudo nmap -sV -Pn -n -p 50000 --source-port 53 10.129.2.47
```

### Result

```text
50000/tcp open tcpwrapped
```

The service could be reached, but Nmap could not determine its version.

This demonstrated that successfully identifying an open port does not guarantee successful service fingerprinting.

### Step 5 — Direct TCP Connection

I attempted to establish a direct connection using Ncat.

The first attempt failed because the local source port was already in use.

I investigated the local socket configuration:

```bash
sudo ss -lntup | grep ':53 '
```

The output showed that `dnsmasq` was using port 53 on another local IP address.

I then specified the appropriate local source address:

```bash
sudo ncat -nv -s 10.10.15.6 -p 53 10.129.2.47 50000
```

### Result

```text
Ncat: Connected to 10.129.2.47:50000.
```

The TCP connection was established successfully.

The captured output confirmed connectivity but did not display a service banner or version.

### Technical Lessons

1. A filtered port may become accessible when different packet characteristics are used.
2. Firewall rules can behave differently depending on the source port.
3. A port number does not guarantee the identity of the running service.
4. Nmap may classify an accessible service as `tcpwrapped` when version detection fails.
5. Local socket configuration can affect testing tools.
6. Accurate enumeration requires interpreting results and adjusting the investigation strategy.

---

## 9. DNS Service Enumeration

Another exercise involved investigating a DNS service protected by filtering rules.

**Target:** `10.129.2.48`

### TCP Scan

```bash
sudo nmap -sV -p 53 10.129.2.48
```

### Result

```text
53/tcp filtered domain
```

### UDP Scan

```bash
sudo nmap -sU -p 53 10.129.2.48
```

### Result

```text
53/udp open domain
```

### DNS Enumeration Technique

```bash
sudo nmap -sU -p 53 --script dns-nsid 10.129.2.48
```

The `dns-nsid` script can retrieve DNS server identification information when the target supports the relevant queries.

### Key Takeaway

DNS commonly operates over both UDP and TCP.

A filtered TCP port does not necessarily mean that the corresponding UDP service is unavailable.

---

## 10. Command Reference

The following commands summarize techniques practiced or discussed during the module.

| Command | Purpose |
|---|---|
| `nmap -sn` | Host discovery |
| `nmap -sS` | TCP SYN scanning |
| `nmap -sA` | TCP ACK scanning |
| `nmap -p-` | Full TCP port scanning |
| `nmap -sV` | Service version detection |
| `nmap -sC` | Default NSE scripts |
| `nmap -O` | Operating system fingerprinting |
| `nmap -sU` | UDP scanning |
| `nmap --script banner` | Service banner enumeration |
| `nmap --script http-enum` | HTTP resource enumeration |
| `nmap --script dns-nsid` | DNS identification queries |
| `nmap --source-port 53` | Source-port-based scanning |
| `nmap --min-rate` | Packet transmission rate configuration |
| `nmap -T4` | Aggressive timing template |
| `curl -I` | HTTP header inspection |
| `ncat -nv` | Direct TCP connection |
| `ss -lntup` | Local socket inspection |

**Note:** This table combines commands executed in practical exercises with techniques studied during the module. It is not intended to represent a chronological execution log.

---

## 11. Final Lessons Learned

Completing this module strengthened my understanding of network reconnaissance and the relationship between scanning tools and network security controls.

The most important lessons were:

**1. Network enumeration requires analytical thinking.**

A scanner provides observations, but the penetration tester must interpret them.

**2. Service identification should be evidence-based.**

Port numbers and default service labels are not sufficient to confirm which application is running.

**3. Firewalls affect network visibility.**

A service may appear filtered under one set of conditions and accessible under another.

**4. Performance optimization involves trade-offs.**

Aggressive scanning may reduce execution time while increasing the risk of incomplete results.

**5. Practical troubleshooting is part of penetration testing.**

Investigating local socket conflicts, interpreting connection errors, and adjusting scan parameters are valuable technical skills.

---

## 12. Module Completion

**Status:** Completed ✅

**Platform:** Hack The Box Academy

**Module:** Network Enumeration with Nmap

**Certification Path:** HTB Certified Penetration Testing Specialist (CPTS)

### Final Reflection

This module reinforced that effective penetration testing depends on understanding network behavior, selecting appropriate tools, and carefully interpreting technical evidence.

The ability to recognize unexpected network responses and investigate their causes is an essential skill for cybersecurity professionals.

**Next Goal:** Continue developing practical offensive security skills through HTB Academy.

---

*All exercises were performed in an authorized training environment provided by Hack The Box Academy.*
