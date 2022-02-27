# Blue Team: Summary of Operations
## **Table of Contents**
- [Network Topology](https://github.com/GarinJTanner/Final-Project/blob/main/Blue-Team.md#network-topology)
- [Description of Targets](https://github.com/GarinJTanner/Final-Project/blob/main/Blue-Team.md#description-of-targets)
- [Monitoring the Targets](https://github.com/GarinJTanner/Final-Project/blob/main/Blue-Team.md#monitoring-the-targets)


## Network Topology  
  
The following machines were identified on the network:

**ML-REFVM-674427**  
- Operating System: Windows 10  
- Purpose: Azure Virtual Environment  
- IP Address: 192.168.1.1  
  
**Kali**  
- Operating System: Kali Linux  
- Purpose: Attack machine  
- IP Address: 192.168.1.90  
  
**ELK**  
- Operating System: Linux  
- Purpose: View web alerts via Kibana  
- IP Address: 192.168.1.100  
  
**Capstone**  
- Operating System: Linux  
- Purpose: Alert testing, attack target  
- IP Address: 192.168.1.105  

**Target 1**
- Operating System: Linux 3.2 - 4.9
- Purpose: WordPress Vulnerability
- IP Address: 192.168.1.110

**Target 2**
- Operating System: Linux
- Purpose: 
- IP Address: 192.168.1.115  
  

## Description of Targets
  The target of this attack was: 192.168.1.110  
  Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented: Excessive HTTP errors, HTTP request size monitor, and CPU Usage Monitor.  
  

## Monitoring the Targets
  Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:  
   
 
**Excessive HTTP Errors**  
- **Metric:** Packetbeat  
- **Threshold:** WHEN count() GROUP OVER top 5 'htttp.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes  
- **Vulnerability Mitigated:** Brute force attacks  
- **Reliability:** Medium/High reliability depending on the amount of traffic coming to the web server. May produce false positives if the only web traffic is coming from a single user mistyping their credentials.  
  

**HTTP Request Size Monitor**  
- **Metric:** Packetbeat  
- **Threshold:** WHEN sum() of 'http.request.bytes' OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute  
- **Vulnerability Mitigated:** DDoS attack  
-  **Reliability:** High reliability. Given the simplicity of the website, typical traffic should not exceed the appointed threshold.   

**CPU Usage Monitor**  
- **Metric:** Metricbeat  
- **Threshold:** WHEN max() OF 'system.process.cpu.total.pct' OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes  
- **Vulnerability Mitigated:** DDoS attack  
- **Reliability:** Low/Medium reliability depending on web traffic.
