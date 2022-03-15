# CICD-SEC-1: Insufficient Flow Control Mechanisms
## Definition

Insufficient flow control mechanisms refer to the ability of an attacker that has obtained permissions to a system within the CI/CD process (SCM, CI, Artifact repository, etc.) to single handedly push malicious code or artifacts down the pipeline, due to a lack in mechanisms that enforce additional approval or review.


## Description

CI/CD flows are designed for speed. New code can be created on a developer’s machine and get to production within minutes, often with full reliance on automation and minimal human involvement. Seeing that CI/CD processes are essentially the highway to the highly gated and secured production environments, organizations continuously introduce measures and controls aimed at ensuring that no single entity (human or application) can push code or artifacts through the pipeline without being required to undergo a strict set of reviews and approvals.


## Impact

An attacker with access to the SCM, CI, or systems further down the pipeline, can abuse insufficient flow control mechanisms to deploy malicious artifacts. Once created, the artifacts are shipped through the pipeline - potentially all the way to production - without any approval or review. For example, an adversary may:



* Push code to a repository branch, which is automatically deployed through the pipeline to production.
* Push code to a repository branch, and then manually trigger a pipeline that ships the code to production.
* Directly push code to a utility library, which is used by code running in a production system.
* Abuse an auto-merge rule in the CI that automatically merges pull requests that meet a predefined set of requirements, thus pushing malicious unreviewed code.
* Abuse insufficient branch protection rules—for example, excluding specific users or branches to bypass branch protection and push malicious unreviewed code.
* Upload an artifact to an artifact repository, such as a package or container, in the guise of a legitimate artifact created by the build environment. In such a scenario, a lack of controls or verifications could result in the artifact being picked up by a deploy pipeline and deployed to production.
* Access production and directly change application code or infrastructure (e.g AWS Lambda function), without any additional approval/verification.


## Recommendations

Establish pipeline flow control mechanisms to ensure that no single entity (human / programmatic) is able to ship sensitive code and artifacts through the pipeline without external verification or validation. This can be achieved by implementing the following measures:



* Configure branch protection rules on branches hosting code which is used in production and other sensitive systems. Where possible, avoid exclusion of user accounts or branches from branch protection rules. Where user accounts are granted permission to push unreviewed code to a repository, ensure those accounts do not have the permission to trigger the deployment pipelines connected to the repository in question.
* Limit the usage of auto-merge rules and ensure that wherever they are in use - they are applicable to the minimal amount of contexts. Review the code of all auto-merge rules thoroughly to ensure they cannot be bypassed and avoid importing 3rd party code in the auto-merge process.
* Where applicable, prevent accounts from triggering production build and deployment pipelines without additional approval or review.
* Prefer allowing artifacts to flow through the pipeline only in the condition that they were created by a pre-approved CI service account. Prevent artifacts that have been uploaded by other accounts from flowing through the pipeline without secondary review and approval.
* Detect and prevent drifts and inconsistencies between code running in production and its CI/CD origin, and modify any resource that contains a drift.


## References



1. Backdoor planted in the PHP git repository. The attackers pushed malicious unreviewed code directly to the PHP main branch, ultimately resulting in a formal PHP version being spread to all PHP websites.

    [https://news-web.php.net/php.internals/113981](https://news-web.php.net/php.internals/113981)

2. Bypassing auto-merge rules in Homebrew, by [RyotaK](https://twitter.com/ryotkak). An auto-merge rule used to merge insignificant changes into the main branch was susceptible to bypass, allowing adversaries to merge malicious code into the project.

    [https://brew.sh/2021/04/21/security-incident-disclosure/](https://brew.sh/2021/04/21/security-incident-disclosure/)

3. Bypassing required reviews using GitHub Actions, by [Omer Gil](https://twitter.com/omer_gil). The flaw allowed leveraging GitHub Actions to bypass the required reviews mechanism and push unreviewed code to a protected branch.

    [https://www.cidersecurity.io/blog/research/bypassing-required-reviews-using-github-actions/](https://www.cidersecurity.io/blog/research/bypassing-required-reviews-using-github-actions/)
