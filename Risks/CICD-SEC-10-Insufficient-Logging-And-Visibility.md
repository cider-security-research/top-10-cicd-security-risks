# CICD-SEC-10: Insufficient Logging and Visibility
## Definition

Insufficient logging and visibility risks allow an adversary to carry out malicious activities within the CI/CD environment without being detected during any phase of the attack kill chain, including identifying the attacker’s TTPs (Techniques, Tactics and Procedures) as part of any post-incident investigation.


## Description

The existence of strong logging and visibility capabilities is essential for an organization’s ability to prepare for, detect and investigate a security related incident.

While workstations, servers, network devices and key IT and business applications are typically covered in depth within an organization’s logging and visibility programs, it is often not the case with systems and processes in engineering environments.

Given the amount of potential attack vectors leveraging engineering environments and processes it is imperative that security teams build the appropriate capabilities to detect these attacks as soon as they happen. As many of these vectors involve leveraging programmatic access against the different systems, a key aspect of facing this challenge is to create strong levels of visibility around both human and programmatic access. 

Given the sophisticated nature of CI/CD attack vectors, there is an equal level of importance to both the audit logs of the systems - e.g. user access, user creation, permission modification, and the applicative logs - e.g. push event to a repo, execution of builds, upload of artifacts. 


## Impact

With adversaries gradually shifting their focus to engineering environments as a means to achieve their goals, organizations which do not ensure the appropriate logging and visibility controls around those environments, may fail to detect a breach, and face great difficulties in mitigation/remediation due to minimal investigative capabilities.

Time and data are the most valuable commodities to an organization under attack. The existence of all relevant data sources in a centralized location may be the difference between a successful and devastating outcome in an incident response scenario.


## Recommendations

There are several elements to achieving sufficient logging and visibility: 



* **Mapping the environment** - Strong visibility capabilities cannot be achieved without an intimate level of familiarity with all the different systems involved in potential threats. A potential breach may involve any of the systems which take part in the CI/CD processes, including SCM, CI, Artifact repositories, package management software, container registries, CD, and orchestration engines (e.g. K8s).

    Identify and build an inventory of all the systems in use within the organization, containing every instance of these systems (specifically relevant for self-managed systems e.g. Jenkins).

* **Identifying and enabling the appropriate log sources** - Once all relevant systems are identified, the next step is ensuring that all relevant logs are enabled, as this is not the default state in the different systems. Visibility should be optimized around both human access as well as programmatic access through all the various measures it is allowed. It is important to place an equal level of emphasis on identifying all relevant audit log sources, as well as the applicative log sources. 
* **Shipping logs to a centralized location** (e.g. SIEM), to support aggregation and correlation of logs between different systems for detection and investigation.
* **Creating alerts to detect anomalies and potential malicious activity**, both in each system on its own and anomalies in the code shipping process, which involves multiple systems and requires deeper knowledge in the internal build and deployments processes.


## References

Logging and visibility capabilities are essential and relevant for being able to detect and investigate any incident, regardless of the risk that was exploited in the incident. Any security incident in recent years involving CI/CD systems required the victim organization to have strong visibility to be able to properly investigate and understand the extent of damage of the attack in question.
