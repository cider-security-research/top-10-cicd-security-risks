# CICD-SEC-8: Ungoverned Usage of 3rd Party Services
## Definition

The CI/CD attack surface consists of an organization’s organic assets, such as the SCM or CI, and the 3rd party services which are granted access to those organic assets. Risks having to do with ungoverned usage of 3rd party services rely on the extreme ease with which a 3rd party service can be granted access to resources in CI/CD systems, effectively expanding the attack surface of the organization.


## Description

It is rare to find an organization which does not have numerous 3rd parties connected to its CI/CD systems and processes. Their ease of implementation, combined with their immediate value, has made 3rd parties an integral part of the engineering day-to-day. The methods of embedding or granting access to 3rd parties are becoming more diverse and the complexities associated with implementing them are diminishing.

Taking a common SCM - GitHub SaaS - as en example, 3rd party applications can be connected through one or more of these 5 methods:



* GitHub Application
* OAuth application
* Provisioning of an access token provided to the 3rd party application
* Provisioning of an SSH key provided to the 3rd party application.
* Configuring webhook events to be sent to the 3rd party.

Each method takes somewhere between seconds and minutes to implement, and grants 3rd parties with numerous capabilities, ranging from reading code in a single repository, all the way to fully administering the GitHub organization. Despite the potentially high level of permission these third parties are granted against the system, in many cases no special permissions or approvals are required by the organization prior to the actual implementation.

Build systems also allow easy integration of 3rd parties. Integrating 3rd parties into build pipelines is usually no more complex than adding 1-2 lines of code within the pipeline configuration file, or installing a plugin from the build system’s marketplace (e.g. actions in Github Actions, Orbs in CircleCI). The 3rd party functionality is then imported and executed as part of the Build process with full access to whatever resources are available from the pipeline stage it is executed in.

Similar methods of connectivity are available in various shapes and forms across most CI/CD systems, creating the process of governing and maintaining least privilege around 3rd party usage across the entire engineering ecosystem extremely complex. Organizations are grappling with the challenge of obtaining full visibility around which 3rd parties have access to the different systems, what methods of access they have, what level of permission/access they have been granted, and what level of permissions/access they are actually using.


## Impact

Lack of governance and visibility around 3rd party implementations prevents organizations from maintaining RBAC within their CI/CD systems. Given how permissive 3rd parties tend to be, organizations are only as secure as the 3rd parties they implement. Insufficient implementation of RBAC and least privilege around 3rd parties, coupled with minimal governance and diligence around the process of 3rd party implementations create a significant increase of the organization’s attack surface.

Given the highly interconnected nature of CI/CD systems and environments, compromise of a single 3rd party can be leveraged to cause damage far outside the scope of the system the 3rd party is connected to (for example, a 3rd party with write permissions on a repository, can be leveraged by an adversary to push code to the repository which will in turn trigger a build and run the adversary’s malicious code on the build system).


## Recommendations

Governance controls around 3rd party services should be implemented within every stage of the 3rd party usage lifecycle:



* **Approval** - Establish vetting procedures to ensure 3rd parties granted access to resources anywhere across the engineering ecosystem are approved prior to being granted access to the environment, and that the level of permission they are granted is aligned with the principle of least privilege.
* **Integration** - Introduce controls and procedures to maintain continuous visibility over all 3rd parties integrated to CI/CD systems, including:
    * Method of integration. Make sure all methods of integration for each system are covered (including marketplace apps, plugins, OAuth applications, programmatic access tokens, etc.).
    * Level of permission granted to the 3rd party.
    * Level of permission actually in use by the 3rd party.
* **Visibility over ongoing usage** - Ensure each 3rd party is limited and scoped to the specific resources it requires access to and remove unused and/or redundant permissions. 3rd parties which are integrated as part of the Build process should run inside a scoped context with limited access to secrets and code, and with strict ingress and egress filters.
* **Deprovisioning** - Periodically review all 3rd parties integrated and remove those no longer in use.


## References



1. Codecov, a popular code coverage tool used in the CI, is compromised to steal environment variables from builds.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

2. Attackers compromise a GitHub user account of a DeepSource (a static analysis platform) engineer. Using the compromised account, they obtain the permissions of the DeepSource GitHub application, granting them full access to the codebase of all DeepSource clients that have installed the compromised GitHub application. 

    [https://discuss.deepsource.io/t/security-incident-on-deepsource-s-github-application/131](https://discuss.deepsource.io/t/security-incident-on-deepsource-s-github-application/131)

3. Attackers gain access to the database of Waydev, a git analytics platform, stealing GitHub and GitLab OAuth tokens of their customers.

    [https://changelog.waydev.co/github-and-gitlab-oauth-security-update-dw98s](https://changelog.waydev.co/github-and-gitlab-oauth-security-update-dw98s)
