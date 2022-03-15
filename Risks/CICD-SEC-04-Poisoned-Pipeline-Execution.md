# CICD-SEC-4: Poisoned Pipeline Execution (PPE)
## Definition


Poisoned Pipeline Execution (PPE) risks refer to the ability of an attacker with access to source control systems - and without access to the build environment, to manipulate the build process by injecting malicious code/commands into the build pipeline configuration, essentially ‘poisoning’ the pipeline and running malicious code as part of the build process.


## Description

The PPE vector abuses permissions against an SCM repository, in a way that causes a CI pipeline to execute malicious commands.

Users that have permissions to manipulate the CI configuration files, or other files which the CI pipeline job relies on, can modify them to contain malicious commands, ultimately “poisoning” the CI pipeline executing these commands.

Pipelines executing unreviewed code, for example those which are triggered directly off of pull requests or commits to arbitrary repository branches, are more susceptible to PPE. The reason is that these scenarios, by design, contain code which has not undergone any reviews or approvals. 

Once able to execute malicious code within the CI pipeline, the attacker can conduct a wide array of malicious operations, all within the context of the pipeline’s identity.

There are three types of PPE:

**Direct PPE (D-PPE):** In a D-PPE scenario, the attacker modifies the CI config file in a repository they have access to, either by pushing the change directly to an unprotected remote branch on the repo, or by submitting a PR with the change from a branch or a fork. Since the CI pipeline execution is triggered off of the “push” or ”PR” events, and the pipeline execution is defined by the commands in the modified CI configuration file, the attacker’s malicious commands ultimately run in the build node once the build pipeline is triggered.

**Indirect PPE (I-PPE):** In certain cases, the possibility of D-PPE is not available to an adversary with access to an SCM repository:



* If the pipeline is configured to pull the CI configuration file from a separate, protected branch in the same repository.
* If the CI configuration file is stored in a separate repository from the source code, without the option for a user to directly edit it.
* If the CI build is defined in the CI system itself - instead of in a file stored in the source code.

In such a scenario, the attacker can still poison the pipeline by injecting malicious code into files referenced by the pipeline configuration file, for example:



* _make_: Executes commands defined in the “Makefile” file.
* Scripts referenced from within the pipeline configuration file, which are stored in the same repository as the source code itself (e.g. _python myscript.py_ - where myscript.py would be manipulated by the attacker).
* Code tests: Testing frameworks running on application code within the build process rely on dedicated files, stored in the same repository as the source code itself. Attackers that are able to manipulate the code responsible for testing are then able to run malicious commands inside the build.
* Automatic tools: Linters and security scanners used in the CI, are also commonly reliant on a configuration file residing in the repository. Many times these configurations involve loading and running external code from a location defined inside the configuration file. 

So rather than poisoning the pipeline by inserting malicious commands directly into the pipeline definition file, In I-PPE, an attacker injects malicious code into files referenced by the configuration file. The malicious code is ultimately executed on the pipeline node once the pipeline is triggered and runs the commands declared in the files in question.

**Public-PPE (3PE):** Execution of a PPE attack requires access to the repository hosting the pipeline configuration file, or to files it references. In most cases, the permission to do so would be given to organization members - mainly engineers. Therefore, attackers would typically have to be in possession of an engineer's permission to the repository to execute a direct or indirect PPE attack.

However, in some cases poisoning CI pipelines is available to anonymous attackers on the internet: Public repositories (for example open source projects) oftentimes allow any user to contribute - usually by creating pull requests, suggesting changes to the code. These projects are commonly automatically tested and built using a CI solution, in a similar fashion to private projects.

If the CI pipeline of a public repository runs unreviewed code suggested by anonymous users, it is susceptible to a Public PPE attack, or in short - 3PE. This also exposes internal assets, such as secrets of private projects, in cases where the pipeline of the vulnerable public repository runs on the same CI instance as private ones.



**Examples**

<span style="text-decoration:underline;">Example 1: Credential theft via Direct-PPE (GitHub Actions)</span>

In the following example, a GitHub repository is connected with a GitHub Actions workflow that fetches the code, builds it, runs tests, and eventually deploys artifacts to AWS.

When new code is pushed to a remote branch in the repository, the code - including the pipeline configuration file - is fetched by the runner (the workflow node).


```
name: PIPELINE
on: push
jobs:
 build:
   runs-on: ubuntu-latest
   steps:
     - run: |
         echo "building..."
         echo "testing..."
         echo "deploying..."
```

![alt_text](../Images/dppe.png "image_tooltip")


In this scenario, a D-PPE attack would be carried out as follows:



1. An attacker creates a new remote branch in the repository, in which they update the pipeline configuration file with malicious commands intended to access AWS credentials scoped to the GitHub organization and then to send them to a remote server.