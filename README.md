# Digital-Forensics-CTF

Digital Forensics Report (Memory Analysis) :
​
- Case : OtterCTF - Phase 1: What is the password?
​
- Investigator: Salim Fouaissi [0x0NullTrace]

​- Tools Used: Volatility Framework 3
​
- Executive Summary :
​
A RAM dump from a suspicious system was analyzed. The primary objective was to identify the user's identity and recover credentials. The password for the user "Rick" was successfully extracted from live memory. Additionally, activity from unknown processes was detected, indicating the potential presence of malware.
​
- System Profile :

​- Based on the analysis of Kernel Structures, the following specifications were identified:

​- Operating System: Windows 7 Service Pack 1 (Build 7601).
​Architecture: x64 (AMD64).

​- Dump Capture Time: 2018-08-04 19:34:22 UTC.
​
- Kernel Base Address: 0xf80002a52000.

​- Technical Investigation :
​
- Process Triage :

​- Upon inspecting the process list (EPROCESS list), anomalies were flagged in the following processes:

​- Rick And Morty (PID 3820): A non-standard process name running under user privileges.

​- LunarMS.exe (PID 708): A suspicious process often associated with external activity.

- ​sc.exe (Multiple PIDs): Frequent and rapid attempts to modify services were observed, suggesting the presence of a malicious automation script.

​- Hash Extraction :

​- The windows.hashdump plugin was utilized to read the SAM and SYSTEM hives.

​- User: Rick
​
- NTLM Hash: 518172d012f97d3a8fcc089615283940
​
- Result: The hash was not found in public Rainbow Tables, necessitating a move toward LSA memory analysis.


​- LSA Secrets Analysis :

​Since the system is Windows 7, the memory of the lsass.exe process was analyzed using the windows.lsadump plugin.
​Location: Evidence was found within LSA Secrets under the DefaultPassword key.

​- Extracted Password: MortyIsReallyAnOtter
​
- Recommendations :
​
- Password Change: The extraction of LSA Secrets implies that an attacker could have obtained full administrative privileges.
​
- Network Analysis: Immediately initiate netscan analysis to determine where the extracted data may have been exfiltrated.
​
- System Upgrade: Migrate from Windows 7 to a modern operating system that implements stronger protections against cleartext password extraction from LSA.


#0x0NullTrace

# 🔍 OtterCTF - Phase 1: What is the password? (Solution)

[![Watch the Video](https://img.youtube.com/vi/OkQx7xlHqGk/maxresdefault.jpg)](https://youtu.be/OkQx7xlHqGk)

### 📝 Quick Overview:
In this write-up, I solve the **OtterCTF** memory forensics challenge using **Volatility 3**. The goal was to extract the user password from a Windows 7 RAM dump.

* **OS:** Windows 7 SP1 (x64)
* **Tools:** Volatility 3
* **Key Finding:** Extracted password `MortyIsReallyAnOtter` from LSA Secrets.

[👉 Click here to watch the full walkthrough on YouTube](https://youtu.be/OkQx7xlHqGk)
