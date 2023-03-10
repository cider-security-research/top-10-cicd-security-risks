# CICD-SEC-3: Dependency Chain Abuse
## Definition


Dependency chain abuse risks refer to an attacker’s ability to abuse flaws relating to how engineering workstations and build environments fetch code dependencies. Dependency chain abuse results in a malicious package inadvertently being fetched and executed locally when pulled.


## Description


Managing dependencies and external packages used by self written code is becoming increasingly complex given the total number of systems involved in the process across all development contexts in an organization. Packages are oftentimes fetched using a dedicated client per programming language, typically from a combination of self-managed package repositories (e.g. Jfrog Artifactory) and language specific SaaS repositories (for example - Node.js has npm and the npm registry, Python’s pip uses PyPI, and Ruby’s gems uses RubyGems).

Many organizations go to great lengths to detect usage of packages with known vulnerabilities and conduct static analysis of both self-written and 3rd party code. However, in the context of using dependencies, there is an equally important set of controls required to secure the dependency ecosystem - involving securing the process defining how dependencies are pulled. Inadequate configurations may cause an unsuspecting engineer, or worse - the build system, to download a malicious package instead of the package that was intended to be pulled. In many cases, the package is not only downloaded, but also immediately executed after download, due to pre-install scripts and similar processes which are designed to run a package’s code immediately after the package is pulled.

The main attack vectors in this context are:



* <span style="text-decoration:underline;">Dependency confusion</span> - Publication of malicious packages in public repositories with the same name as internal package names, in an attempt to trick clients into downloading the malicious package rather than the private one.
* <span style="text-decoration:underline;">Dependency hijacking</span> - Obtaining control of the account of a package maintainer on the public repository, in order to upload a new, malicious version of a widely used package, with the intent of compromising unsuspecting clients who pull the latest version of the package.
* <span style="text-decoration:underline;">Typosquatting</span> - Publication of malicious packages with similar names to those of popular packages in the hope that a developer will misspell a package name and unintentionally fetch the typosquatted package.
* <span style="text-decoration:underline;">Brandjacking</span> - Publication of malicious packages in a manner that is consistent with the naming convention or other characteristics of a specific brand’s package, in an attempt to get unsuspecting developers to fetch these packages due to falsely associating them with the trusted brand.


## Impact

The objective of adversaries which upload packages to public package repositories using one of the aforementioned techniques is to execute malicious code on a host pulling the package. This could either be a developer’s workstation, or a build server pulling the package.

Once the malicious code is running, it can be leveraged for credentials theft and lateral movement within the environment it is executed in.

Another potential scenario is for the attacker’s malicious code to make its way to production environments from the build server. In many cases the malicious package would continue to also maintain the original, safe functionality the user was expecting, resulting in a lower probability of discovery. 


## Recommendations

There is a wide range of mitigation methods which are specific to the configuration of the different language-specific clients and the way internal proxies and external package repositories are used.

That said, all recommended controls share the same guiding principles -



* Any client pulling code packages should not be allowed to fetch packages directly from the internet or untrusted sources. Instead, the following controls should be implemented:
    * Whenever 3rd party packages are pulled from an external repository, ensure all packages are pulled through an internal proxy rather than directly from the internet. This allows deploying additional security controls at the proxy layer, as well as providing investigative capabilities around packages pulled - in case of a security incident. 
    * Where applicable, disallow pulling of packages directly from external repositories. Configure all clients to pull packages from internal repositories, containing pre-vetted packages, and establish a mechanism to verify and enforce this client configuration.
* Enable checksum verification and signature verification for pulled packages. 
* Avoid configuring clients to pull the latest version of a package. Prefer configuring a pre-vetted version or version ranges. Use the framework specific techniques to continuously “lock” the package version required in your organization to a stable and secure version.
* Scopes:
    * Ensure all private packages are registered under the organization’s scope.
    * Ensure all code referencing a private package uses the package’s scope. 
    * Ensure clients are forced to fetch packages that are under your organization’s scope solely from your internal registry.
* When installation scripts are being executed as part of the package installation, ensure that a separate context exists for those scripts, which does not have access to secrets and other sensitive resources available in other stages in the build process.
* Ensure that internal projects always contain configuration files of package managers (for example .npmrc in NPM) within the code repository of the project, to override any insecure configuration potentially existing on a client fetching the package.
* Avoid publishing names of internal projects in public repositories.
* As a general rule, given the amount of package managers and configurations in use simultaneously, complete prevention of 3rd party chain abuse is far from trivial. It is therefore recommended to ensure that an appropriate level of focus is placed around detection, monitoring and mitigation to ensure that in case of an incident, it is identified as quickly as possible and has the minimal amount of potential damage. In this context, all relevant systems should be hardened properly according to the guidelines under the “CICD-SEC-7: Insecure System Configuration” risk.


## References



1. Dependency Confusion, by [Alex Birsan](https://twitter.com/alxbrsn). An attack vector that tricks package managers and proxies into fetching a malicious package from a public repository instead of the intended package of the same name from an internal repository.

    [https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)

2. Amazon, Zillow, Lyft, and Slack NodeJS apps targeted by threat actors using the Dependency Confusion vulnerability.

    [https://www.bleepingcomputer.com/news/security/malicious-npm-packages-target-amazon-slack-with-new-dependency-attacks/](https://www.bleepingcomputer.com/news/security/malicious-npm-packages-target-amazon-slack-with-new-dependency-attacks/)

3. The _ua-parser-js_ NPM library, with 9 million downloads a week, was hijacked to launch cryptominers and steal credentials.

    [https://github.com/advisories/GHSA-pjwm-rvh2-c87w](https://github.com/advisories/GHSA-pjwm-rvh2-c87w)

4. The _coa_ NPM library, with 9 million downloads a week, was hijacked to steal credentials.

    [https://github.com/advisories/GHSA-73qr-pfmq-6rp8](https://github.com/advisories/GHSA-73qr-pfmq-6rp8)

5. The _rc_ NPM library, with 14 million downloads a week, was hijacked to steal credentials.

    [https://github.com/advisories/GHSA-g2q5-5433-rhrf](https://github.com/advisories/GHSA-g2q5-5433-rhrf)
    
6. Sonartype researchers uncover two malicious typosquat packages was used to hijack sensitive information and publish to a public GitHub page.

    [https://blog.sonatype.com/sonatype-spots-malicious-npm-packages](https://blog.sonatype.com/sonatype-spots-malicious-npm-packages)
