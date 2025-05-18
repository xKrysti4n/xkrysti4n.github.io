# 🧠 Red Team Roadmap

## 🟢 FAZA 1:[ Fundamenty Web i HTTP](../fundamenty_html/)  (priorytet: WYSOKI) 
- [x] Co to jest HTTP / HTTPS  
- [x] Czym jest request i response  
- [x] Metody HTTP: GET, POST, PUT, DELETE, OPTIONS, HEAD  
- [ ] Nagłówki HTTP: User-Agent, Host, Cookie, Referer, Origin, Authorization  
- [ ] Status codes: 200, 301, 302, 403, 404, 401, 500  
- [ ] Cookies i sesje  
- [ ] CORS (Cross-Origin Resource Sharing)  
- [ ] Same-Origin Policy  
- [ ] Znajomość jak działa przeglądarka i serwer  

📚 Materiały:  

- [ ] [PortSwigger Web Security Academy](https://portswigger.net/web-security)

- [ ] TryHackMe – Web Fundamentals / OWASP Top 10 / Burp Suite

- [ ] https://developer.mozilla.org/en-US/docs/Web/HTTP (MDN)

---

## 🟡 FAZA 2: Web Application Attacks (OWASP Top 10)

- [ ] Injection → SQLi, Command Injection, LDAP
- [ ] XSS → Stored, Reflected, DOM
- [ ] Broken Authentication
- [ ] Insecure Direct Object Reference (IDOR)
- [ ] Security Misconfiguration
- [ ] CSRF
- [ ] SSTI
- [ ] File Upload Attacks
- [ ] Open Redirect
- [ ] Clickjacking
- [ ] Rate limiting / bruteforce bypasses

📚 Materiały:  

- [ ] PortSwigger: każdy temat + laby  
- [ ] TryHackMe OWASP Top 10  
- [ ] Burp Suite Academy Labs  
- [ ] [PayloadsAllTheThings GitHub](https://github.com/swisskyrepo/PayloadsAllTheThings)

---

## 🔵 FAZA 3: Narzędzia i praktyka (PO KURSIE PORTSWIGGER)

- [ ] Burp Suite: Proxy, Repeater, Intruder, Decoder, Extender
- [ ] Dirbusting (gobuster / ffuf / feroxbuster)
- [ ] Subdomain enumeration (amass, assetfinder, subfinder)
- [ ] Wappalyzer / WhatWeb
- [ ] Nmap + podstawy rekonesansu
- [ ] Brute force z Hydra / wfuzz
- [ ] Hashcat: cracking podstawy (maski, słowniki, rules)
- [ ] Znajomość popularnych CMS (WordPress, Joomla, itp.)

---

## 🧨 FAZA 4: Kurs pełny – **Practical Ethical Hacking (TCM)**

- [ ] Recon (passive/active)
- [ ] Scanning & enumeration
- [ ] Exploitation – Linux & Windows
- [ ] Pivoting & tunneling (chisel, SSH socks)
- [ ] Password attacks
- [ ] Active Directory Basics
- [ ] Enumeration with tools (CrackMapExec, BloodHound)

📚 Kurs:  

- [ ] [TCM Security - Practical Ethical Hacking (PEH)](https://academy.tcm-sec.com/p/practical-ethical-hacking-the-complete-course)

---

## 🔴 FAZA 5: Zaawansowany Red Team / Real Labs

- [ ] Active Directory Attacks (kerberoasting, AS-REP roasting, etc.)
- [ ] Windows Post Exploitation
- [ ] Credential dumping (Mimikatz, LSASS, SAM hive)
- [ ] C2 Frameworks (Sliver, Mythic, Havoc)
- [ ] AV / EDR evasion (basic opsec)
- [ ] Enumeracja sieci i lateral movement (PsExec, WMI, etc.)

📚 Materiały:  

- [ ] HTB Academy (Windows Internals, AD, Red Team Path)  
- [ ] TryHackMe – Red Team & AD rooms  
- [ ] PortSwigger: Web Logic flaws, JWT, OAuth  
- [ ] Red Team Notes – [https://github.com/0xpgp/Red-Teaming-Notes](https://github.com/0xpgp/Red-Teaming-Notes)

---

## ⚫ FAZA 6 (Opcjonalna): Reverse Engineering & Exploity (po kilku miesiącach)

- [ ] Ghidra / GDB / pwndbg / radare2
- [ ] Stack buffer overflows
- [ ] Format string bugs
- [ ] Shellcode
- [ ] ASLR / DEP / PIE / NX bypass

📚 Kursy:  

- [ ] Pwn College  
- [ ] TryHackMe: Binary Exploitation  
- [ ] TCM - Intro to Malware Development

---

## 🧰 LAB SETUP

- [ ] Kali Linux / Arch for Pentest
- [ ] VirtualBox / KVM
- [ ] Active Directory lab (Windows Server + Win10 clients)
- [x] Burp Suite (Community / Pro)
