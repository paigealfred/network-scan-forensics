# Network Scan Forensics Investigation

## Overview

PCAP analysis investigating network reconnaissance activity using TCP stream reconstruction. Traced scan results from compromised host to external C2 server and identified the final target IP before connection termination.

## Scenario

A compromised internal host (172.16.100.55) reported network scan results to an external IP (103.69.117.6). Investigation required extracting the complete scan session and identifying the last IP address scanned.

**Challenge Question:** What was the very last IP that scan results were sent to before the connection was closed?

**Answer:** 172.16.100.253

## Tools Used

- **Docket** - Targeted PCAP extraction and filtering
- **Wireshark** - Packet analysis and protocol investigation
- **TCP Stream Analysis** - Session reconstruction

## Investigation Process

### 1. Traffic Isolation
- Switched from Kibana to Docket for packet-level control
- Filtered for traffic between 172.16.100.55 and 103.69.117.6
- Generated clean PCAP with only relevant communication

### 2. PCAP Analysis
- Opened PCAP in Wireshark
- Identified TCP connections carrying scan data
- Applied "Follow TCP Stream" to reconstruct session

### 3. Intelligence Extraction
- Reviewed complete TCP stream in ASCII format
- Scrolled to end of stream (final data before disconnect)
- Identified last IP transmitted: **172.16.100.253**

## Key Findings

**Compromised Host:** 172.16.100.55  
**C2 Server:** 103.69.117.6  
**Final Scan Target:** 172.16.100.253  
**Attack Pattern:** Network reconnaissance → scan result exfiltration → potential lateral movement preparation

## MITRE ATT&CK Mapping

- **T1046** - Network Service Discovery
- **T1071** - Application Layer Protocol (C2)
- **T1041** - Exfiltration Over C2 Channel
- **T1595** - Active Scanning

## Technical Skills Demonstrated

✅ Targeted PCAP filtering using Docket  
✅ Wireshark packet analysis  
✅ TCP stream reconstruction  
✅ Traffic correlation between specific hosts  
✅ C2 communication analysis  
✅ Network forensics methodology  
✅ Intelligence extraction from packet data  

## Why This Matters

**Investigative Value:**
- Identified scope of network reconnaissance
- Revealed C2 infrastructure
- Determined extent of scanning activity
- Provided IOCs for threat hunting

**Actionable Intelligence:**
- Block C2 IP: 103.69.117.6
- Isolate compromised host: 172.16.100.55
- Investigate if 172.16.100.253 was subsequently compromised
- Hunt for additional reconnaissance activity

## Key Techniques

**Docket for Noise Reduction:**
- Extracted only relevant traffic between two hosts
- Created manageable PCAP for detailed analysis
- Better control than SIEM for packet-level investigation

**TCP Stream Following:**
- Reconstructed complete application-layer communication
- Revealed scan results in readable format
- Identified final action before connection termination

## Lessons Learned

- PCAP analysis provides ground truth when SIEM logs are insufficient
- TCP stream reconstruction reveals complete attacker activity
- Final data before disconnect often indicates next attack stage
- Targeted filtering accelerates forensic analysis
- Understanding protocol behavior is critical for C2 detection

---

**Investigation Date:** Lab Environment  
**Tools:** Docket, Wireshark  
**Analyst:** Paige Alfred
