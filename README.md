# 🧪 Windows Event Log Analysis Lab

## 📝 Introduction

This lab simulates a real-world **brute-force attack scenario**. The investigation begins with Windows systems, then pivots to a compromised Linux system (`linux-programatic-vr-ena`). By following the **suspicious login story arc**, I demonstrate end-to-end investigative thinking and **KQL-based detection** across operating systems using Microsoft Sentinel.

---

## 🛠️ Tools Used

- Microsoft Sentinel  
- Azure Virtual Machines (Windows & Linux)  
- Kusto Query Language (KQL)

---

## 🔍 Suspicious Login Story Arc: A Lab Walkthrough

### Step 1: 🚫 Brute-Force Login Attempts

**Query:** `Brute Force Login Detection`  
**Outcome:**  
- Detected 100+ failed login attempts from account `root` on multiple Linux systems  
- Detected 70+ failed attempts against the `administrator` account on several Windows VMs

**🧠 Insight:**  
This pattern suggests **brute-force activity**, often used for initial access. It highlights the importance of:
- Disabling unused accounts  
- Enforcing strong password policies  
- Implementing account lockout mechanisms

📸 *Screenshot:* `Brute Force Query Result`

---

### Step 2: ✅ Successful Login After Failures

**Query:** `Successful Login After Failures`  
**Outcome:**  
- `root` account on `linux-programatic-vr-ena` failed 100 times and then **logged in successfully** at 3:55 PM (4 minutes after last failure)

**🧠 Insight:**  
This suggests a **likely brute-force compromise** and would warrant escalation to **incident response** in a real SOC environment.

📸 *Screenshot:* `Successful Login After Failures`

---

### Step 3: 🧨 Suspicious Post-Login Command Execution

**Query:** `Suspicious Process Execution`  
**Outcome:**  
- Post-login, `root` launched several `bash` and `sh` sessions  
- Many linked to the `/waagent/` directory, suggesting script execution or system tampering

**🧠 Insight:**  
Indicates likely **attacker-driven command execution**. The `/waagent/` activity may signal payload staging or post-exploitation via the Azure guest agent.

📸 *Screenshot:* `Suspicious Process Execution`

---

### Step 4: 🌐 External Network Communication

**Query:** `External Network Communication`  
**Outcome:**  
- `root` account initiated outbound connections over **port 443**
- HTTP requests to **external IPs** followed shortly after shell activity

**🧠 Insight:**  
Outbound activity by a compromised root account implies **C2 (Command and Control)** or **payload retrieval**. Justifies:
- IP blocking  
- Host isolation  
- Forensic analysis

📸 *Screenshot:* `External Network Communication`

---

## 🧩 MITRE ATT&CK Techniques Observed

| Technique ID | Name |
|--------------|------|
| T1110.001 | Brute Force: Password Guessing |
| T1059.004 | Command and Scripting Interpreter: Unix Shell |
| T1071.001 | Application Layer Protocol: Web Protocols |

---

## 🧠 Lessons Learned

This lab demonstrated a **full attack lifecycle**:
- Brute-force login attempts  
- System compromise  
- Post-login exploitation  
- External communication

### 🔑 Key Takeaways

- **Login Events Matter:** High-volume failures followed by success = red flag  
- **Post-Login Activity Is Critical:** Watch command execution after login  
- **External Communications Tell the Final Story:** They may reveal malware downloads, C2, or data exfiltration

---

## 🕵️ Analyst Mindset

This exercise reinforced how a SOC analyst must:

- Correlate data from **LogonEvents → ProcessEvents → NetworkEvents**
- Think like a **threat hunter**
- Tell a **cohesive incident story**
- Use Microsoft Sentinel like a professional SOC analyst

---

## ✅ Demonstrated Skills

- KQL for real-world security investigation  
- Cross-platform incident analysis (Windows & Linux)  
- Clear, documented detection and investigation workflow  
- Threat hunting and timeline correlation  
- Entry-level SOC operations using Microsoft Sentinel
