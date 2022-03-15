# CICD-SEC-5: Insufficient PBAC (Pipeline-Based Access Controls)
## Definition

Pipeline execution nodes have access to numerous resources and systems within and outside the execution environment. When running malicious code within a pipeline, adversaries leverage insufficient PBAC (Pipeline-Based Access Controls) risks to abuse the permission granted to the pipeline for moving laterally within or outside the CI/CD system. 


## Description

Pipelines are the beating heart of CI/CD. Nodes executing pipelines carry out the commands specified in the pipeline configuration and by doing so - conduct a wide array of sensitive activities:



* Access source code, build and test it.
* Obtain secrets from various locations, such as environment variables, vaults, dedicated cloud-based identity services (such as the AWS metadata service), and other locations.
* Create, modify and deploy artifacts.

PBAC is a term which refers to the context in which each pipeline - and each step within that pipeline - is running. Given the highly sensitive and critical nature of each pipeline, it is imperative to limit each pipeline to the exact set of data and resources it needs access to. Ideally, each pipeline and step should be restricted in such a manner that will ensure that in case an adversary is able to to execute malicious code within the context of the pipeline, the extent of potential damage is minimal.

PBAC includes controls relating to numerous elements having to do with the pipeline execution environment:



* Access within the pipeline execution environment: to code, secrets, environment variables, and other pipelines.
* Permissions to the underlying host and other pipeline nodes.
* Ingress and egress filters to the internet. 


## Impact

A piece of malicious code that is able to run in the context of the pipeline execution node has the full permissions of the pipeline stage it runs in. It can access secrets, access the underlying host and connect to any of the systems the pipeline in question has access to. This can lead to exposure of confidential data, lateral movement within the CI environment - potentially accessing servers and systems outside the CI environment, and deployment of malicious artifacts down the pipeline, including to production.

The extent of the potential damage of a scenario in which an adversary is able to compromise pipeline execution nodes or inject malicious code into the Build process is determined by the granularity of the PBAC in the environment.

## Recommendations



* Do not use a shared node for pipelines with different levels of sensitivity / that require access to different resources. Shared nodes should be used only for pipelines with identical levels of confidentiality.
* Ensure secrets that are used in CI/CD systems are scoped in a manner that allows each pipeline and step to have access to only the secrets it requires.
* Revert the execution node to its pristine state after each pipeline execution.
* Ensure the OS user running the pipeline job has been granted OS permissions on the execution node according to the principle of least privilege.
* CI and CD pipeline jobs should have limited permissions on the controller node. Where applicable, run pipeline jobs on a separate, dedicated node.
* Ensure the execution node is appropriately patched.
* Ensure network segmentation in the environment the job is running on is configured to allow the execution node to access only the resources it requires within the network. Where possible, refrain from granting unlimited access towards the internet to build nodes.
* When installation scripts are being executed as part of the package installation, ensure that a separate context exists for those scripts, which does not have access to secrets and other sensitive resources available in other stages in the build process.


## References



1. Codecov, a popular code coverage tool used in the CI, was compromised and used to steal environment variables from builds.

    [https://about.codecov.io/security-update/](https://about.codecov.io/security-update/)

2. Amazon, Zillow, Lyft, and Slack NodeJS apps targeted by threat actors using the Dependency Confusion vulnerability. Organizations that were victims of Dependency Confusion attacks had malicious code executed on CI nodes, allowing the adversary to move laterally within the environment and abuse insufficient PBAC.

    [https://www.bleepingcomputer.com/news/security/malicious-npm-packages-target-amazon-slack-with-new-dependency-attacks/](https://www.bleepingcomputer.com/news/security/malicious-npm-packages-target-amazon-slack-with-new-dependency-attacks/)

3. A vulnerability found in Teleport’s CI implementation, allowed attackers from the internet to execute a Direct-3PE attack to run a privileged container and escalate to root privilege on the node itself - leading to secret exfiltration, release of malicious artifacts, and access to sensitive systems.

    [https://goteleport.com/blog/hack-via-pull-request/](https://goteleport.com/blog/hack-via-pull-request/)
