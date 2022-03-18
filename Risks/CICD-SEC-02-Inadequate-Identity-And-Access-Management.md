# CICD-SEC-2: Inadequate Identity and Access Management
## Definition


Inadequate Identity and Access Management risks stem from the difficulties in managing the vast amount of identities spread across the different systems in the engineering ecosystem, from source control to deployment. The existence of poorly managed identities - both human and programmatic accounts - increases the potential and the extent of damage of their compromise. 


## Description

Software delivery processes consist of multiple systems connected together with the aim of moving code and artifacts from development to production. Each system provides multiple methods of access and integration (username & password, personal access token, marketplace application, oauth applications, plugins, SSH keys). The different types of accounts and method of access can potentially have their own unique provisioning method, set of security policies and authorization model. This complexity creates challenges in managing the different identities throughout the entire identity lifecycle and ensuring their permissions are aligned with the principle of least privilege.

Furthermore, in a typical environment, the average user account of an SCM or CI is highly permissive, as these systems have not traditionally been a major focus area for security teams. These identities are mostly used by engineers that require the flexibility to be able to create major changes in code and infrastructure. 

Some of the major concerns and challenges around identity and access management within the CI/CD ecosystem include:



* **Overly permissive identities** - Maintaining the principle of least privilege for both applicative and human accounts. For example, in SCMs - Ensuring each human and applicative identity has been granted only the permissions required and only against the actual repositories it needs to access is not trivial.
* **Stale identities** - Employees/Systems that are not active and/or no longer require access but have not had their human and programmatic account against all CI/CD systems deprovisioned.
* **Local identities** - Systems which do not have their access federated with a centralized IDP, creating identities that are managed locally within the system in question. Local accounts create challenges in enforcing consistent security policies (e.g. password policy, lockout policy, MFA) as well as properly deprovisioning access across all systems (for example, when an employee leaves the organization).
* **External identities** - 
    * <span style="text-decoration:underline;">Employees registered with an email address from a domain not owned or managed by the organization</span> - In this scenario, the security of these accounts is highly dependent on the security of the external accounts they are assigned to. Since these accounts are not managed by the organization, they are not necessarily compliant with the organization's security policy. 
    * <span style="text-decoration:underline;">External collaborators</span> - Once access is granted to external collaborators to a system, the security level of the system is derived from the level of the external collaborator’s work environment, outside of the organization’s control.
* **Self-registered identities** - In systems where self-registration is allowed, it is often the case that a valid domain address is the only prerequisite for self-registration and access to CI/CD systems. Usage of default/base set of permissions to a system which is anything different than “none” significantly expands the potential attack surface.
* **Shared identities** - Identities shared between human users / applications / both humans and applications increase the footprint of their credentials as well as create challenges having to do with accountability in case of a potential investigation. 


## Impact

The existence of hundreds (or sometimes thousands) of identities - both human and programmatic - across the CI/CD ecosystem, paired with a lack of strong identity and access management practices and common usage of overly permissive accounts, leads to a state where compromising nearly any user account on any system, could grant powerful capabilities to the environment, and could serve as a segway into the production environment.


## Recommendations



* Conduct a continuous analysis and mapping of all identities across all systems within the engineering ecosystem. For each identity, map the identity provider, level of permissions granted and level of permissions actually used. Ensure all methods of programmatic access are covered within the analysis.
* Remove permissions not necessary for the ongoing work of each identity across the different systems in the environment.
* Determine an acceptable period for disabling/removing stale accounts and disable/remove any identity which has surpassed the predetermined period of inactivity.
* Avoid creating local user accounts. Instead, create and manage identities using a centralized organization component (IdP). Whenever local user accounts are in use, ensure that accounts which no longer require access are disabled/removed and that security policies around all existing accounts match the organization’s policies.
* Continuously map all external collaborators and ensure their identities are aligned with the principle of least privilege. Whenever possible, grant permissions with a predetermined expiry date - for both human and programmatic accounts - and disable their account once the work is done.
* Prevent employees from using their personal email addresses, or any address which belongs to a domain not owned and managed by the organization, against the SCM, CI, or any other CI/CD platform. Continuously monitor for non-domain addresses across the different systems and remove non-compliant users.
* Refrain from allowing users to self-register to systems, and grant permission on an as-needed basis.
* Refrain from granting base permissions in a system to all users, and to large groups where user accounts are automatically assigned to.
* Avoid using shared accounts. Create dedicated accounts for each specific context, and grant the exact set of permissions required for the context in question.


## References



1. The Stack Overflow TeamCity build server compromise - The attacker was able to escalate their privileges in the environment due to the fact the newly registered accounts were assigned administrative privileges upon access to the system. 

    [https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/](https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/)

2. Mercedes Benz source code leaked after a self-maintained internet-facing GitLab server was available for access by self-registration.

    [https://www.zdnet.com/article/mercedes-benz-onboard-logic-unit-olu-source-code-leaks-online/](https://www.zdnet.com/article/mercedes-benz-onboard-logic-unit-olu-source-code-leaks-online/)

3. A self-managed GitLab server of the New York state government was exposed to the internet, allowing anyone to self-register and log in to the system, which stored sensitive secrets.

    [https://techcrunch.com/2021/06/24/an-internal-code-repo-used-by-new-york-states-it-office-was-exposed-online/](https://techcrunch.com/2021/06/24/an-internal-code-repo-used-by-new-york-states-it-office-was-exposed-online/)

4. Malware added to the Gentoo Linux distribution source code, after the GitHub account password of a project maintainer was compromised.

    [https://wiki.gentoo.org/wiki/Project:Infrastructure/Incident_Reports/2018-06-28_Github](https://wiki.gentoo.org/wiki/Project:Infrastructure/Incident_Reports/2018-06-28_Github)
