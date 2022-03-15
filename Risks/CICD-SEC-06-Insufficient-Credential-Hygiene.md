# CICD-SEC-6: Insufficient Credential Hygiene
## Definition


Insufficient credential hygiene risks deal with an attacker’s ability to obtain and use various secrets and tokens spread throughout the pipeline due to flaws having to do with access controls around the credentials, insecure secret management and overly permissive credentials.


## Description

CI/CD environments are built of multiple systems communicating and authenticating against each other, creating great challenges around protecting credentials due to the large variety of contexts in which credentials can exist.

Application credentials are used by the application at runtime, credentials to production systems are used by pipelines to deploy infrastructure, artifacts and apps to production, engineers use credentials as part of their testing environments and within their code and artifacts.

This variety of contexts, paired with the large amount of methods and techniques for storing and using them, creates a large potential for insecure usage of credentials. Some major flaws that affect credential hygiene:



* **Code containing credentials being pushed to one of the branches of an SCM repository:** This can be either by mistake - without noticing the existence of the secret in the code, or deliberately - without understanding the risk of doing that. From that moment on, the credentials are exposed to anyone with read access to the repository, and even if deleted from the branch it was pushed into - they continue to appear in the commit history, available to be viewed by anyone with repository access.
* **Credentials used insecurely inside the build and deployment processes:** These credentials are used to access code repositories, read from and write to artifact repositories, and deploy resources and artifacts to production environments. Given the large amount of pipelines and target systems they need access to, it’s imperative to understand - 
    * In which context, and using which method, is each set of credentials used? 
    * Can each pipeline access only the credentials it needs to fulfill its purpose?
    * Can credentials be accessed by unreviewed code flowing through the pipeline?
    * How are these credentials called and injected to the build? Are these credentials accessible only in run-time, and only from the contexts where they are required?
* **Credentials in container image layers:** Credentials that were only required for building the image, still exist in one of the image layers - available to anyone who is able to download the image.
* **Credentials printed to console output:** Credentials used in pipelines are often printed to the console output, deliberately or inadvertently. This might leave credentials exposed in clear-text in logs, available to anyone with access to the build results to view. These logs can potentially flow to log management systems, expanding their exposure surface.
* **Unrotated credentials:** Since the credentials are spread all over the engineering ecosystem, they are exposed to a large number of employees and contractors. Failing to rotate credentials results in a constantly growing amount of people and artifacts that are in possession of valid credentials. This is especially true for credentials used by pipelines - for example deploy keys - which are oftentimes managed using the “If it isn’t broken, don’t fix it” directive - which leaves valid credentials unrotated for many years.


## Impact

Credentials are the most sought-after object by adversaries, seeking to use them for accessing high-value resources or for deploying malicious code and artifacts. In this context, engineering environments provide attackers with multiple avenues to obtain credentials. The large potential for human error, paired with knowledge gaps around secure credentials management and the concern of breaking processes due to credential rotation, put the high-value resources of many organizations at the risk of compromise due the exposure of their credentials.


## Recommendations



* Establish procedures to continuously map credentials found across the different systems in the engineering ecosystem - from code to deployment. Ensure each set of credentials follows the principle of least privilege and has been granted the exact set of permission needed by the service using it. 
* Avoid sharing the same set of credentials across multiple contexts. This increases the complexity of achieving the principle of least privilege as well as having a negative effect on accountability. 
* Prefer using temporary credentials over static credentials. In case static credentials need to be in use - establish a procedure to periodically rotate all static credentials and detect stale credentials. 
* Configure usage of credentials to be limited to predetermined conditions (like scoping to a specific source IP or identity) to ensure that even in case of compromise, exfiltrated credentials cannot be used outside your environment.
* Detect secrets pushed to and stored on code repositories. Use controls such as an IDE plugin to identify secrets used in the local changes, automatic scanning upon each code push, and periodical scans on the repository and its past commits.
* Ensure secrets that are used in CI/CD systems are scoped in a manner that allows each pipeline and step to have access to only the secrets it requires.
* Use built-in vendor options or 3rd party tools to prevent secrets from being printed to console outputs of future builds. Ensure all existing outputs do not contain secrets.
* Verify that secrets are removed from any type of artifact, such as from layers of container images, binaries, or Helm charts.


## References



1. Thousands of credentials, stored as environment variables, were stolen by attackers through compromising Codecov, a popular code coverage tool used in the CI.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

2. Travis CI injected secure environment variables of public repositories into pull request builds, causing them to be susceptible to compromise by anonymous users issuing pull requests against public repositories. 

    [https://travis-ci.community/t/security-bulletin/12081](https://travis-ci.community/t/security-bulletin/12081)

3. An attacker compromised the TeamCity Build server of Stack Overflow and was able to steal secrets due to their insecure storage method.

    [https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/](https://stackoverflow.blog/2021/01/25/a-deeper-dive-into-our-may-2019-security-incident/)

4. Samsung exposed overly permissive secrets in public GitLab repositories.

    [https://techcrunch.com/2019/05/08/samsung-source-code-leak/](https://techcrunch.com/2019/05/08/samsung-source-code-leak/)

5. Attackers accessed Uber’s private GitHub repositories that contained permissive and shared AWS tokens, leading to data exfiltration of millions of drivers and passengers. [https://www.ftc.gov/system/files/documents/federal_register_notices/2018/04/152_3054_uber_revised_consent_analysis_pub_frn.pdf](https://www.ftc.gov/system/files/documents/federal_register_notices/2018/04/152_3054_uber_revised_consent_analysis_pub_frn.pdf)
6. Gaining write access to Homebrew, by Eric Holmes. The Homebrew Jenkins instance revealed environment variables of executed builds, including a GitHub token which allowed an attacker to make malicious changes to the Homebrew project itself.

    [https://medium.com/@vesirin/how-i-gained-commit-access-to-homebrew-in-30-minutes-2ae314df03ab](https://medium.com/@vesirin/how-i-gained-commit-access-to-homebrew-in-30-minutes-2ae314df03ab)
