## **Mindset for entire phase (non-negotiable):**

> _You are not learning tools. You are learning how attacks unfold in real organizations._

---

## 🧠 OVERALL STRUCTURE


- 🎯 Goal
    
- 🧩 What to learn
    
- ⚔️ What to practice
    
- 🚫 What to avoid
    
- 🔥 Danger level (how real the skills are)
    

---

# 🟥 MONTH 1 — INITIAL ACCESS MASTERY (FOUNDATION)

### 🎯 Goal

Become **reliable at getting initial access** without touching exploits.

This is where **most real breaches start**.

---

## 1️⃣ Email & Human Attack Surface (EASY → MEDIUM)

### Learn:

- Phishing psychology (urgency, authority, curiosity)
    
- Pretext creation (HR, IT support, invoices)
    
- Email headers (SPF, DKIM, DMARC basics)
    
- HTML email anatomy
    
- Payload vs link-based attacks
    

### Practice:

- Build **fake but realistic email templates**
    
- Create phishing flows (open → click → payload)
    
- GoPhish campaigns (local lab first)
    
- QR phishing logic (Quishing)
    

### Add:

- **HTML Smuggling** (critical modern technique)
    

🔥 Danger level: **Low → Medium**  
(Realistic but controlled)

---

## 2️⃣ Malicious Documents & Files (MEDIUM)

### Learn:

- VBA macros (basic → obfuscated)
    
- DDE attack logic (not tool copy)
    
- LNK file internals
    
- ISO/IMG delivery
    
- Office template injection
    

### Practice:

- Create **one macro payload manually**
    
- Build **one LNK payload**
    
- Test delivery on lab VM
    

🚫 Avoid:

- “One-click exploit PDFs”
    
- Tool-only payload generators at this stage
    

🔥 Danger level: **Medium**

---

## 3️⃣ External Hardware Attacks (OPTIONAL but valuable)

### Learn:

- HID keystroke attacks
    
- USB execution behavior
    
- Human trust exploitation
    

### Practice:

- Rubber Ducky logic (not mass deployment)
    

🔥 Danger level: **Medium**

---

# 🟧 MONTH 2 — PRIVILEGE ESCALATION + LOCAL DOMINANCE

### 🎯 Goal

Turn **initial foothold → full system control** reliably.

---

## 4️⃣ Linux Privilege Escalation (EASY → HARD)

### Learn:

- SUID / SGID logic
    
- Cron abuse
    
- Writable config abuse
    
- PATH hijacking
    
- Kernel exploit theory (no rush)
    

### Practice:

- Manual privesc **before using LinPEAS**
    
- Then validate with tools
    

🔥 Danger level: **Medium**

---

## 5️⃣ Windows Privilege Escalation (MEDIUM → HARD)

### Learn:

- Services & permissions
    
- AlwaysInstallElevated
    
- Unquoted paths
    
- DLL hijacking
    
- UAC bypass internals
    

### Practice:

- One **manual service abuse**
    
- One **DLL hijack**
    
- One **UAC bypass**
    

🚫 Avoid:

- Running WinPEAS blindly without understanding output
    

🔥 Danger level: **High**

---

## 6️⃣ Local Persistence (IMPORTANT)

### Learn:

- Scheduled tasks
    
- Registry run keys
    
- Service persistence
    
- WMI event subscriptions
    

### Practice:

- Create persistence
    
- Remove persistence
    
- Detect persistence
    

🔥 Danger level: **High**

---

# 🟨 MONTH 3 — LATERAL MOVEMENT + ACTIVE DIRECTORY WARFARE

### 🎯 Goal

Move **inside networks like an operator**, not a CTF player.

---

## 7️⃣ Lateral Movement (MEDIUM)

### Learn:

- SMB authentication flow
    
- Pass-the-Hash vs Pass-the-Ticket
    
- WMI vs PsExec vs WinRM
    
- Credential reuse logic
    

### Practice:

- One host → another host
    
- Credential capture → reuse
    

🔥 Danger level: **High**

---

## 8️⃣ Pivoting & Internal Recon (MEDIUM → HARD)

### Learn:

- SOCKS proxies
    
- Chisel architecture
    
- SSH tunnels
    
- Internal port scanning
    

### Practice:

- Route traffic through compromised host
    
- Scan internal-only services
    

🔥 Danger level: **High**

---

## 9️⃣ Active Directory Attacks (HARD)

### Learn:

- AD structure deeply (users, groups, trusts)
    
- Kerberos fundamentals
    
- Ticket lifecycle
    
- Delegation & AdminSDHolder
    

### Practice:

- BloodHound analysis
    
- Kerberoasting
    
- AS-REP roasting
    
- DCSync (lab only)
    

🚫 Avoid:

- Running Mimikatz without knowing _why_
    

🔥 Danger level: **Very High**

---

# 🟩 MONTH 4 — C2, OPSEC, REAL RED TEAM MODE

### 🎯 Goal

Operate **quietly, stealthily, professionally**.

This month separates **hackers from red teamers**.

---

## 🔟 Command & Control (HARD)

### Learn:

- Beaconing logic
    
- Transport protocols
    
- Stagers vs full payloads
    
- Infrastructure OPSEC
    

### Practice:

- One C2 framework deeply (not all)
    
- HTTP + DNS beaconing
    
- Memory-only execution
    

🔥 Danger level: **Very High**

---

## 1️⃣1️⃣ OPSEC & Defense Evasion (VERY HARD)

### Learn:

- AMSI bypass logic
    
- ETW patching
    
- Inline syscalls
    
- EDR evasion philosophy
    

### Practice:

- Obfuscation (PowerShell / Python / Nim)
    
- Sleeping beacons
    
- Parent PID spoofing
    

🚫 Avoid:

- “Bypass packs” without understanding
    

🔥 Danger level: **EXTREME**

---

## 1️⃣2️⃣ Full Red Team Simulation (FINAL)

### Do:

- Initial Access → PrivEsc → Pivot → AD → C2 → Cleanup
    
- Document every step
    
- Map to MITRE ATT&CK
    

This is where **everything merges**.

🔥 Danger level: **REAL OPERATOR**