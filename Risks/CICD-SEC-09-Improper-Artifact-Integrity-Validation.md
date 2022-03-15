# CICD-SEC-9: Improper Artifact Integrity Validation
## Definition


Improper artifact integrity validation risks allow an attacker with access to one of the systems in the CI/CD process to push malicious (although seemingly benign) code or artifacts down the pipeline, due to insufficient mechanisms for ensuring the validation of code and artifacts. 


## Description

CI/CD processes consist of multiple steps, ultimately responsible for taking code all the way from an engineer’s workstation to production. There are multiple resources being fed into each step - combining internal resources and artifacts with 3rd party packages and artifacts fetched from remote locations. The fact that the ultimate resource is reliant upon multiple sources spread across the different steps, provided by multiple contributors, creates multiple entry points through which this ultimate resource can be tampered with. 

If a tampered resource was able to successfully infiltrate the delivery process, without raising any suspicion or encountering any security gates - it will most likely continue flowing through the pipeline - all the way to production - in the guise of a legitimate resource.


## Impact

Improper artifact integrity validation can be abused by an adversary with a foothold within the software delivery process to ship a malicious artifact through the pipeline, ultimately resulting in the execution of malicious code - either on systems within the CI/CD process or worse - in production.


## Recommendations

The prevention of improper artifact integrity validation risks requires a collection of measures, across different systems and stages within the software delivery chain. Consider the following controls: 



* Implement processes and technologies to validate the integrity of resources all the way from development to production. When a resource is generated, the process will include signing that resource using an external resource signing infrastructure. Prior to consuming the resource in subsequent steps down the pipeline, the resource’s integrity should be validated against the signing authority. Some prevalent measures to consider in this context:
    * **Code signing** - SCM solutions provide the ability to sign commits using a unique key for each contributor. This measure can then be leveraged to prevent unsigned commits from flowing down the pipeline.
    * **Artifact verification software** - Usage of tools for signing and verification of code and artifacts provide a way to prevent unverified software from being delivered down the pipeline. An example for such a project is Sigstore, created by the Linux Foundation.
    * **Configuration drift detection** - Measures aimed at detecting configuration drifts (e.g. resources in cloud environments which aren’t managed using a signed IAC template), potentially indicative of resources that were deployed by an untrusted source or process.
* 3rd party resources fetched from build/deploy pipelines (such as scripts imported and executed as part of the build process) should follow a similar logic - prior to using 3rd party resources, the hash of the resource should be calculated and cross referenced against the official published hash of the resource provider. 


## References



1. The hack of the SolarWinds build system, used to spread malware through SolarWinds to 18,000 organizations. The code of the Orion software was changed in the build system during the build process, leaving no trace in the codebase.

	[https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm](https://sec.report/Document/0001628280-20-017451/#swi-20201214.htm)



2. Codecov, a popular code coverage tool used in the CI, is compromised to steal environment variables from builds. Attackers gained access to the GCP (Google Cloud Platform) account hosting the Codecov script, and modified it to contain malicious code. The attack was identified by a customer comparing the hash of the script stored on GitHub with the script downloaded from the GCP account.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

3. Backdoor planted in the PHP git repository, ultimately resulting in a formal PHP version being spread to all PHP users. The attackers push malicious unreviewed code directly to the PHP main branch, committing the code as if it were made by known PHP contributors.

    [https://news-web.php.net/php.internals/113981](https://news-web.php.net/php.internals/113981)

4. Attackers compromise the Webmin build server, and add a backdoor to one of the application’s scripts. The backdoor continued to persist even after the compromised build server was decommissioned due to the fact that code was restored from a local backup, rather than the source control system. Webmin users were susceptible to RCE through a supply chain attack for a duration of over 15 months, until the backdoor was removed.

    [https://www.webmin.com/exploit.html](https://www.webmin.com/exploit.html)
