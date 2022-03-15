# Top 10 CI/CD Security Risks
## Introduction

CI/CD environments, processes, and systems are the beating heart of any modern software organization. They deliver code from an engineer’s workstation to production. Combined with the rise of the DevOps discipline and microservice architectures, CI/CD systems and processes have reshaped the engineering ecosystem:

* The technical stack is more diverse, both in relation to coding languages as well as to technologies and frameworks adopted further down the pipeline (e.g. GitOps, K8s).
* Adoption of new languages and frameworks is increasingly quicker, without significant technical barriers.
* There is an increased use of automation and Infrastructure as Code (IaC) practices.
* 3rd parties, both in the shape of external providers as well as dependencies in code, have become a major part of any CI/CD ecosystem, with the integration of a new service typically requiring no more than adding 1-2 lines of code.

These characteristics allow faster, more flexible and diverse software delivery. However, they have also reshaped the attack surface with a multitude of new avenues and opportunities for attackers.

Adversaries of all levels of sophistication are shifting their attention to CI/CD, realizing CI/CD services provide an efficient path to reaching an organization’s crown jewels. The industry is witnessing a significant rise in the amount, frequency and magnitude of incidents and attack vectors focusing on abusing flaws in the CI/CD ecosystem, including - 

* The compromise of the **SolarWinds** build system, used to spread malware through to 18,000 customers.
* The **Codecov** breach, that led to exfiltration of secrets stored within environment variables in thousands of build pipelines across numerous enterprises.
* The **PHP breach**, resulting in publication of a malicious version of PHP containing a backdoor.
* The **Dependency Confusion** flaw, which affected dozens of giant enterprises, and abuses flaws in the way external dependencies are fetched to run malicious code on developer workstations and build environments.
* The compromises of the **_ua-parser-js_, _coa_ and _rc_ NPM packages**, with millions of weekly downloads each, resulting in malicious code running on millions of build environments and developer workstations.

While attackers have adapted their techniques to the new realities of CI/CD, most defenders are still early on in their efforts to find the right ways to detect, understand, and manage the risks associated with these environments. Seeking the right balance between optimal security and engineering velocity, security teams are in search for the most effective security controls that will allow engineering to remain agile without compromising on security.


##  The “Top 10 CI/CD Security Risks” initiative

This document helps defenders identify focus areas for securing their CI/CD ecosystem. It is the result of extensive research into attack vectors associated with CI/CD, and the analysis of high profile breaches and security flaws.

Numerous industry experts across multiple verticals and disciplines came together to collaborate on this document to ensure its relevance to today’s threat landscape, risk surface, and the challenges that defenders face in dealing with these risks.

We would like to thank and acknowledge all experts which took part in reviewing and validating this document:
