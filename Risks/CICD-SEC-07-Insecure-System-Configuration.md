# CICD-SEC-7: Insecure System Configuration
## Definition


Insecure system configuration risks stem from flaws in the security settings, configuration and hardening of the different systems across the pipeline (e.g. SCM, CI, Artifact repository), often resulting in “low hanging fruits” for attackers looking to expand their foothold in the environment.


## Description

CI/CD environments are comprised of multiple systems, provided by a variety of vendors. To optimize CI/CD security, defenders are required to place strong emphasis both on the code and artifacts flowing through the pipeline, and the posture and resilience of each individual system.

In a similar way to other systems storing and processing data, CI/CD systems involve various security settings and configurations on all levels - application, network and infrastructure. These settings have a major influence on the security posture of the CI/CD environments and the susceptibility to a potential compromise. Adversaries of all levels of sophistication, are always on the lookout for potential CI/CD vulnerabilities and misconfigurations that can be leveraged to their benefit.

Examples of potential hardening flaws:



* A self-managed system and/or component using an outdated version or lacking important security patches.
* A system having overly permissive network access controls. 
* A self-hosted system that has administrative permissions on the underlying OS.
* A system with insecure system configurations. Configurations typically determine key security features having to do with authorization, access controls, logging and more. In many cases, the default set of configurations is not secure and requires optimization. 
* A system with inadequate credential hygiene - for example default credentials which are not disabled, overly permissive programmatic tokens, and more. 

While usage of SAAS CI/CD solutions, rather than their self-hosted alternative, eliminates some of the potential risks associated with system hardening and lateral movement within the network, organizations are still required to be highly diligent in securely configuring their SAAS CI/CD solution. Each solution has its own set of unique security configurations and best practices which are essential for maintaining optimal security posture.


## Impact

A security flaw in one of the CI/CD systems may be leveraged by an adversary to obtain unauthorized access to the system or worse - compromise the system and access the underlying OS. These flaws may be abused by an attacker to manipulate legitimate CI/CD flows, obtain sensitive tokens and potentially access production environments. In some scenarios, these flaws may allow an attacker to move laterally within the environment and outside the context of CI/CD systems. 


## Recommendations



* Maintain an inventory of systems and versions in use, including mapping of a designated owner for each system. Continuously check for known vulnerabilities in these components. If a security patch is available, update the vulnerable component. If not, consider removing the component / system, or reduce the potential impact of exploiting the vulnerability by restricting access to the system, or the system’s ability to perform sensitive operations.
* Ensure network access to the systems is aligned with the principle of least access. 
* Establish a process to periodically review all system configurations for any setting that can have an effect on the security posture of the system, and ensure all settings are optimal.
* Ensure permissions to the pipeline execution nodes are granted according to the principle of least privilege. A common misconfiguration in this context is around granting debug permissions on execution nodes to engineers. While in many organizations this is a common practice, it is imperative to take into consideration that any user with the ability to access the execution node in debug mode may expose all secrets while they are loaded into memory and use the node’s identity - effectively granting elevated permissions to any engineer with this permission.


## References



1. The compromise of the SolarWinds build system, used to spread malware through SolarWinds to 18,000 organizations.

	[https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm](https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm)



2. Backdoor planted in the PHP git repository. The attackers pushed malicious unreviewed code directly to the PHP main branch, ultimately resulting in a formal PHP version being spread to all PHP users. The attack presumably originated in a compromise of the PHP self-maintained git server.

    [https://news-web.php.net/php.internals/113981](https://news-web.php.net/php.internals/113981)

3. An attacker compromised Stack Overflow’s TeamCity build server, which was accessible from the internet.

    [https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/](https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/)

4. Attackers compromised an unpatched Webmin build server, and added a backdoor to the local copy of the code after being fetched from the repository, leading to a supply chain attack on servers using Webmin.

    [https://www.webmin.com/exploit.html](https://www.webmin.com/exploit.html)

5. Nissan source code leaked after a self-managed Bitbucket instance left accessible from the internet with default credentials.

    [https://www.zdnet.com/article/nissan-source-code-leaked-online-after-git-repo-misconfiguration/](https://www.zdnet.com/article/nissan-source-code-leaked-online-after-git-repo-misconfiguration/)

6. Mercedes Benz source code leaked after a self-maintained internet-facing GitLab server was made open for self-registration.

    [https://www.zdnet.com/article/mercedes-benz-onboard-logic-unit-olu-source-code-leaks-online/](https://www.zdnet.com/article/mercedes-benz-onboard-logic-unit-olu-source-code-leaks-online/)

7. A self-managed GitLab server of the New York state government was exposed to the internet, allowing anyone to self-register and log in to the system, which stored sensitive secrets.

    [https://techcrunch.com/2021/06/24/an-internal-code-repo-used-by-new-york-states-it-office-was-exposed-online/](https://techcrunch.com/2021/06/24/an-internal-code-repo-used-by-new-york-states-it-office-was-exposed-online/)
