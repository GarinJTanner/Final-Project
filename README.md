## Blue Team: Summary of Operations
-Table of Contents
-Network Topology
-Description of Targets
-Monitoring the Targets
-Patterns of Traffic & Behavior
-Suggestions for Going Further
-Network Topology

The following machines were identified on the network:

ML-REFVM-674427
  Operating System: Windows 10
  Purpose: Azure Virtual Environment
  IP Address: 192.168.1.1
  
Kali
  Operating System: Kali Linux
  Purpose: Attack machine
  IP Address: 192.168.1.90
  
ELK
  Operating System: Linux
  Purpose: View web alerts via Kibana
  IP Address: 192.168.1.100
  
Capstone
  Operating System: Linux
  Purpose: Alert testing, attack target
  IP Address: 192.168.1.105

Target 1
  Operating System: Linux 3.2 - 4.9
  Purpose: WordPress Vulnerability
  IP Address: 192.168.1.110

Target 2
  Operating System: Linux
  Purpose: 
  IP Address: 192.168.1.115

### Description of Targets
  The target of this attack was: 192.168.1.110
  Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented: Excessive HTTP errors, HTTP request size monitor, and CPU Usage Monitor.


### Monitoring the Targets
  Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:
Excessive HTTP Errors

Alert 1 is implemented as follows:
Metric: WHEN max() OF system.process.cpu.total.pct OVER all documents
Threshold: IS ABOVE 0.5 FOR THE LAST 5 minutes
Vulnerability Mitigated: Brute force attacks
Reliability: Medium reliability depending on the amount of traffic coming to the web server. May produce false positives if the only web traffic is coming from a single user mistyping their credentials. 
HTTP Request Size Monitor

Alert 2 is implemented as follows:
Metric: WHEN sum() of http.request.bytes OVER all documents
Threshold: IS ABOVE 3500 FOR THE LAST 1 minute
Vulnerability Mitigated: Brute force
Reliability: High reliability. Given the simplicity of the website, typical traffic should not exceed the appointed threshold.
CPU Usage Monitor

Alert 3 is implemented as follows:
Metric: WHEN max() OF system.process.total.pct OVER all documents
Threshold: IS ABOVE 0.5 FOR THE LAST 5 minutes
Vulnerability Mitigated: TODO
Reliability: TODO: Does this alert generate lots of false positives/false negatives? Rate as low, medium, or high reliability.
TODO Note: Explain at least 3 alerts. Add more if time allows.
Suggestions for Going Further (Optional)
TODO:
Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain how to implement each patch.
The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:
Vulnerability 1
Patch: TODO: E.g., install special-security-package with apt-get
Why It Works: TODO: E.g., special-security-package scans the system for viruses every day
Vulnerability 2
Patch: TODO: E.g., install special-security-package with apt-get
Why It Works: TODO: E.g., special-security-package scans the system for viruses every day
Vulnerability 3
Patch: TODO: E.g., install special-security-package with apt-get
Why It Works: TODO: E.g., special-security-package scans the system for viruses every day

