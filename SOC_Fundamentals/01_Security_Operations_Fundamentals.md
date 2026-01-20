# Security Operations
Aim is to understand the core functions of a SOC role, daily tasks and responsibilities, understand models, roles and structures.
Learn metrics which evaluate the effectiveness of security operations, understand tools for monitoring, detections, analysis, and response. Identify common threats.

## The SOC and its role
 - Monitor
 - Detect
 - Analyze
 - Respond

## Information Security Refresher
CIA Triad - Confidentiality, Integrity, Availability
AAA Framework - Authentication, Authorization, Accounting

Risk control strategies:
1. Risk transference (insurance)
2. Risk acceptance 
3. Risk avoidance
4. Risk mitigation

Security policies:
1. AUP - acceptable use policy - what is allowed within an organization
2. Password policy -  storing, reuse, complexity
3. Data classification 
4. Change management - approving any changes
5. Disaster recovery policy.

## SOC Models
- Internal SOC - fully integrated into organization's infrastructure. Comes with a lot of investment.
- Managed SOC - third-party, basic monitoring, lack of transparency and communication, subscription-based
- Hybrid SOC - a mix of internal and managed. Flexible, call experts when needed.

SOC Roles

**SOC Analysts**:
		Tier 1 Analyst - initial triage
		Tier 2 Analyst - incident responder, investigating and remediating
		Tier 3 Analyst - threat hunters, more of a proactive role.
	SOC Team lead(not always)

**Specialized roles**. Those you can progress to throughout your career.
	Incident responder - dedicated team responding to security incidents.
	Threat hunters - developing custom alerts, looking for undetected threats and risks.
	Threat Intelligence Analyst - gathering and analyzing questionable intelligence. OSINT
	Security Engineer - ppl who design and implement security controls, technologies. Configuring firewalls and IDSs.
	Vulnerability manager.
	Forensic analyst - analyzing digital evidence. 
	Malware Analyst - analyze and reverse engineer malware.


**Management roles**

SOC Manager - managing personnel, activities and budget.
Director of Security - providing strategic decisions, standards, policies.
CISO - Chief Information Security Officer - responsible for organization's security.

## Incident and Event Management
 - Collection, normalization, analysis
 - Logs, alerts, endpoints
 - Identify abnormal or suspicious activities
 - Security Information and Event Management

## SOC Metrics
"If you can't measure it - you can't improve it" - Peter Drucker, american management consultant, author

 - Mean Time To Detect
	Lower MTTD - faster detection
 - Mean Time to Resolution
	Lower MTTR - more efficient IR
 - Mean Time to Attend and Analyze
	Lower MTTA&A - lower response latency
 - Incident Detection Rate
	Higher rate - better visibility and monitoring
 - False Positive Rates
	Lower rate - more accurate detection
 - False Negative Rates
	Lower rate - more accurate detection

## SOC Tools
- Security Information and Event Management -collecting, analyzing in real time
- Security Automation, Orchestration, and Response(SOAR) -automated processes
- Incident Management Tools:
	- Incident Ticketing
	- Alert Management
	- Workflow automation
	- Collaboration
	Network Security Monitoring - packet capture and analysis, traffic analysis, intrusion detection
	IDS/IPS - detect, take actions. 
	EDR - 
		Real-time endpoint monitoring
		User Entity Behavior Analysis(UEBA)
		Threat Detection and Prevention
		Incident investigation
		Remediation and Response
		Integration with SIEM

## Firewalls
- Network Firewalls
	Examine packets
	Layer 3
	Make decisions based on rules
- Next Generation Firewalls (NGFW)
	Stateful packet inspection
	Deep packet inspection
	Layer 7
- Web Application Firewall
	Inspect HTTP(S) traffic
	Protect web applications from attacks
	Layer 7

## Common Threats and Attacks
https://attack.mitre.org/groups/

1. Social Engineering
	Phishing
	Spoofing
	Vishing
	Smishing
	Quishing
2. Malware
	Worm, Spyware, Adware
	Ransomware
	Trojan
	Fileless Malware
3. Identity and Account Compromise
	Credential Theft
4. Insider Threats
5. Advanced Persistent Threats(APTs)
6. DOS Attacks
7. Data Breaches
8. 0-days
9. Supply Chain Attacks