# networkwalks-B082-week2-Phase1-2
Week 2 - Footprinting &amp; Network Scanning project for Networkwalks Cybersecurity Internship B082



**Program/Batch:** B082-Networkwalks
**Date:** 23 August 2026
**Modules completed:** W2-PM1 (Multiple Kali Tools) + W2-PM5 (Zenmap Network Scanning)

---

## 1. Introduction

This project covers the first phase of penetration testing: **Footprinting & Reconnaissance**, followed by **Network Scanning**. The goal is to passively gather public information about the target `networkwalks.com`, then perform active host discovery and port scanning on the local lab network.

---

## 2. Tools Used

| Tool | Purpose |
|---|---|
| whois | Domain registration lookup |
| whatweb | Web technology fingerprinting |
| nslookup | DNS resolution |
| curl | HTTP header inspection |
| wafw00f | WAF detection |
| dnsrecon | DNS enumeration |
| Zenmap/Nmap | Network scanning & host discovery |

---

## 3. Activities Performed

### 3.1 Task 1 – whois
Command: `whois networkwalks.com`

Findings: Domain registered via GoDaddy.com, LLC, created on 06-Nov-2019, expires 06-Nov-2027. Name servers point to HostGator (NS6135/NS6136.HOSTGATOR.COM), confirming the hosting provider. Registrant identity protected via Domains By Proxy.

<img width="959" height="463" alt="whoissreen" src="https://github.com/user-attachments/assets/6e7cfda4-8c65-4d32-8a6f-3a581b64c587" />


---

### 3.2 Task 2 – whatweb
Command: `whatweb networkwalks.com`

Findings: Site runs Apache + WordPress 7.0.4 with WordPress Download Manager plugin 3.3.58. Server IP identified as 192.232.216.135. Contact email exposed: info@networkwalks.com.

<img width="959" height="458" alt="whatweb_image" src="https://github.com/user-attachments/assets/61f256b1-7ff5-44db-a2ad-70467c66bb7a" />



---

### 3.3 Task 3 – nslookup
Command: `nslookup networkwalks.com`

Findings: Domain resolves to IP 192.232.216.135, consistent with whatweb results.

<img width="956" height="469" alt="nslookup" src="https://github.com/user-attachments/assets/40daa532-314a-4887-b889-4d4ac9aec135" />


---

### 3.4 Task 4 – curl
Command: `curl -I https://networkwalks.com`

Findings: HTTP/2 200 OK response. Server: Apache. WordPress REST API endpoint exposed (`/wp-json/`), confirming CMS in use. Secure HttpOnly cookie in use.

<img width="959" height="503" alt="curl" src="https://github.com/user-attachments/assets/70ea3615-4b25-43c9-b303-afd7a7e35645" />


---

### 3.5 Task 5 – wafw00f
Command: `wafw00f networkwalks.com`

Findings: The site is protected by **ModSecurity (SpiderLabs) WAF**, meaning naive attack attempts would likely be blocked or logged.

<img width="773" height="413" alt="wafw00f" src="https://github.com/user-attachments/assets/d55e3e67-d668-42f9-8325-42e494727384" />


---

### 3.6 Task 6 – dnsrecon
Command: `dnsrecon -d networkwalks.com`

Findings: Full DNS footprint mapped — SOA/NS servers (HostGator), MX record (mail.networkwalks.com), SPF policy, and 8 SRV records for cPanel email autodiscovery. Recursion was found enabled on the name servers.

<img width="959" height="302" alt="dnsrecon" src="https://github.com/user-attachments/assets/9b76fe72-7094-4582-a612-5efe03f5497b" />


---

### 3.7 Network Scanning – Zenmap (W2-PM5)
Command: `nmap -A 192.168.209.0/24` (Intense Scan via Zenmap, own lab subnet)

Findings: 4 active hosts discovered out of 255 scanned. Host `192.168.209.1` (own PC/host machine) exposed SMB ports (135/139/445) and an open PostgreSQL port (5432). Host `192.168.209.2` is the NAT DNS gateway (port 53). Hosts `.254` and `.131` (lab VM) had all ports filtered by firewall — good security posture.

<img width="959" height="415" alt="zenmap" src="https://github.com/user-attachments/assets/f316924f-8dc6-4a3c-83ac-813dd2279acc" />


<img width="959" height="488" alt="Znmap1" src="https://github.com/user-attachments/assets/2c1a20d4-ebbd-4434-95aa-6586a7e569e0" />



---

## 4. Key Takeaways

- Passive recon alone revealed the hosting provider, CMS, server IP, WAF, and full DNS map of the target — without ever touching it directly.
- Active scanning on the lab subnet showed how easily open services (SMB, PostgreSQL) can be discovered, highlighting the importance of minimizing exposed ports.
- A WAF (ModSecurity) adds a layer of defense that any attacker must account for before attempting further attack phases.

---

## 5. Liability Disclaimer

This project was conducted strictly for **educational purposes** as part of the Networkwalks Cybersecurity Internship (Batch B082). All reconnaissance was performed against `networkwalks.com` (Networkwalks' own public site, testing authorized) and my own local lab network. No unauthorized systems were tested.
