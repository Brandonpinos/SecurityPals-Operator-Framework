# Security Self Assessment for Open Source Project
### Course Code: LFEL1005
The content of this guide has been taken from the Linux Foundation course on "Security Self-Assessments for Open Source Projects"
For more information: [click here](https://training.linuxfoundation.org/express-learning/security-self-assessments-for-open-source-projects-lfel1005/)
The examples are rudimentary and only offers a simple guide. For more thorough examples, look at the following 


A security self-assessment is a process that helps you understand your current security situation. It's like taking a good look at your security measures and figuring out what's working and what needs to be improved. This process is beneficial because:

- It gives those in charge a better understanding of the current security situation.
- It helps identify areas that need improvement, making it easier to enhance security.
- It keeps everyone involved in the loop about how security measures are progressing.
- It makes future assessments quicker and easier by documenting responses to frequently asked security questions.

For more information on threat models that can be improved through security self-assessment, check out this [repository]('argoproj/docs/end_user_threat_model.pdf').

Here are the items we would be creating in the security self-assessment of a given project:
- Metadata: Security Links
- Overview: Actors, Actions, Background, Goals, and Non-Goals
- Self-Assessment Use
- Security Functions and Features
- Project Compliance
- Secure Development Practices
- Security Issue Resolution
- Appendix


## Metadata Contents

The first thing you'll need for your self-assessment is the metadata. This is a quick overview of your project:

- **Software**: Include a link to your software's repository.
- **Security Provider**: Is the main purpose of your project to enhance the security of an integrated system? (Yes/No). For example, if your project is an authentication provider.
- **Languages**: What languages is your project written in?
- **Software Bill of Materials (SBOM)**: Provide a link to the libraries, packages, and versions used by your project. This may also include direct dependencies.
- **Security Links**: Include a list of links to any existing security documentation for your project.

Look at two of the ways you could implement this section below:

Example 1:---------------------------------------------------------------------------------------

  ### Software
  - https://privateerproj/privateer
  - https://privateerproj/privateer-sdk
  - https://privateerproj/privateer-raid-wireframe
  ### Security Provider?
  No. Privateer is designed to facilitate security or compliance validation, but it should not be considered a security provider.
  ### Languages
  Go
  ### Software Bill of Materials
  Known Weakness. Automated generation of each repo's SBOM is not yet complete, and should be added to the roadmap.
  ### Security Links
  Known Weakness. Creation of a security-insights.yml should be added to the roadmap.
  
OR,


Example 2:---------------------------------------------------------------------------------------

  
  ## Metadata
  | | |
  |-----------|------|
  | Software | https://privateerproj/privateer<‌br‌>https://privateerproj/privateer-sdk<‌br‌>https://privateerproj/privateer-raid-wireframe |
  | Security Provider? | No. Privateer is designed to facilitate security or compliance validation, but it should not be considered a security provider. |
  | Languages | Go |
  | Software Bill of Materials | Known Weakness. Automated generation of each repo's SBOM is not yet complete, and should be added to the roadmap. |
  | Security Links | Known Weakness. Creation of a security-insights.yml should be added to the roadmap. |

End of example:----------------------------------------------------------------------------------


## Overview
The overview statement is a basic summary of the project, and has multiple components nested beneath it.


Example:-----------------------------------------------------------------------------------------

  ## Overview
  Privateer is a test harness that is specially designed to combine any number of validation test packs into a single runtime that will harmonize inputs, executions, and outputs. The Privateer SDK enables the creation of test plugins, nicknamed _raids_, which can be selected and executed by Privateer users one at a time or in groups.

End of Example:----------------------------------------------------------------------------------

The second part is the background, which is an explanation behind the motivation of the existence of your project. 

Example:-----------------------------------------------------------------------------------------
 
  ### Background
  Historically, runtime validation tests are notoriously specific to the resource they're validating. While the validators are powerful, they typically only address a single use case.
  In situations where engineers need to validate a wide array of deployed resources, they must build a custom solution that incorporates the commands and configurations for each validation tool.
  Privateer seeks to remediate this problem by allowing validation tests to be built as "raids" which receive their configuration from the Privateer executable, and subsequently pass their outputs back to the Privateer. When multiple raids are executed by a user, only a single config is required, all executions log their status together, and the output will always be provided together in a matching format.


End of Example:----------------------------------------------------------------------------------


The third part is the actors, i.e., different parts of the project that act upon each other. Different actors should ideally be isolated, and the means by which they are isolated should also be described.

Example:-----------------------------------------------------------------------------------------

  ### Actors
  1. The core executable, [Privateer](htt‌ps://github.com/privateerproj/privateer), serves to initialize all plugins. This process will continue running concurrently while sequentially initializing Raids as secondary processes.
  2. Raid plugins, built in the style of the [raid wireframe](ht‌tps://github.com/privateerproj/privateer-raid-wireframe), are executed independently of the core process and are fully unaware of other Raid processes.

End of Example:----------------------------------------------------------------------------------


The fourth part is about actions. This includes:
- Details about how your actors act.
- After documenting the Actors, it’s time to show what their Actions are. This is a high-level description of how the actors interact with each other.
- If you have a more complex system, you may want to create a chart using a tool such as draw.io (the system used for the purpose of this demonstration has relatively low complexity, so the following example may be a bit overkill).
- If you find another way to communicate this for your situation, do what you think will best aid your readers in understanding the actions


Example:----------------------------------------------------------------------------------

### Actions
The Privateer Core will read the user config to determine which Raids are to be read, and customize the runtime behavior based on options such as loglevel.
One at a time, Privateer will initialize Raids by calling the respective subprocess by name, based on user input. Each Raid subprocess will independently read the configuration file for information such as runtime customization or resource authentication values.
The Privateer SDK enables each Raid to print and write logs or output for the execution. Upon completion of all Raids, Privateer will print a summary of the Raid results.
![Flowchart of the actors discussed above](actors_actions.png)

End of Example-----------------------------------------------------------------------------


The fifth part is your project goals.This describes what your project intends to do, and what security considerations are intended.

While the Overview and Background sections help establish the core features of the project, the Goals section should describe what the project intends to accomplish. This includes both the end user value and the security goals for the project.

A way to start thinkng about what the security goals might be would be to think about the points where the software in question would do any of the following:
- Touch the internet
- Receive untrusted input
- Handle sensitive data

Example-------------------------------------------------------------------------------------

### Goals
The Privateer project intends to create an ecosystem of post-deployment validation tools that can be easily incorporated into any automation pipeline.
In order to mitigate the risk of compromised open source dependencies, Privateer ensures that sensitive information stored within a configuration may be fully isolated between plugins ("Raids") when multiple implementations are executed simultaneously.


End of example-------------------------------------------------------------------------------


The upcoming section, titled "Non-Goals", outlines the security aspects that are not intended to be covered by this project. These are specific security features that a user might assume to be included, but the project has deliberately deemed them out of scope for valid reasons. 

These non-goals could be answers to questions that have already been asked, but they also aim to preemptively address potential criticisms and queries from users or security auditors.

In the following example, a few items warranted individual explanations, so they each have their own subheadings. You might prefer to use a list or other format to organize your non-goals.

It's important to provide as much detail as possible in this section. Clearly state the concerns and then thoroughly address them. Remember, the goal is to make this as intuitive and understandable as possible for the reader.

Example-------------------------------------------------------------------------------------

### Non-Goals
The Privateer project does not maintain packages, instead relying on the greater open source community to maintain each package independently of the main project. Privateer does not attempt to validate the security of plugins that are requested by users.
#### Plugin Validation
The Privateer project does not currently provide validation or assurances of safety for plugins ("Raids") that users choose to execute.
A roadmap candidate is being considered to address this by offering a "--safe" flag to the CLI, which will only execute plugins that are retrieved from an official source. This is currently not a goal due to the large scope of the process: Adding this feature will require (at minimum) an approval process, a list or registry of approved plugins, and automated provenance validation built into the --safe flag.
For the foreseeable future, Privateer will treat all plugins selected by users as fully-trusted entities.
#### Subprocess Command to execute plugins
When Privateer calls the raids as subprocesses, no validation is performed to restrict subprocesses to safe executables. To exploit this, the Privateer executable or respective configuration file must already be compromised by an attacker. In either case, there is no additional opportunity provided to attackers by restricting subprocess commands further.

End of example-------------------------------------------------------------------------------


This information will help your stakeholders understand your project at a high level, removing the need for a variety of follow-up questions later on. This contextualization is especially important when working with a joint-reviewer or auditor

The “Self-Assessment Use” section is designed to provide readers with a clear understanding of the purpose and motivation behind the creation of the document. It delves into the background and driving forces that led to the development of the document, offering a glimpse into the thought process and intentions of the creators.

This section also outlines the expected usage of the document by the readers. It provides guidance on how to best utilize the information presented, and what outcomes or benefits the readers can anticipate from its use.


Example-------------------------------------------------------------------------------------


## Self-assessment Use
This self-assessment is created by the Privateer team to perform an internal analysis of the project's security. It is not intended to provide a security audit of Privateer, or function as an independent assessment or attestation of Privateer's security health.
This document serves to provide Privateer users with an initial understanding of Privateer's security, where to find existing security documentation, Privateer plans for security, and general overview of Privateer security practices, both for development of Privateer as well as security of Privateer.
This document provides Privateer maintainers and stakeholders with additional context to help inform the roadmap creation process, so that security and feature improvements can be prioritized accordingly.



End of example-------------------------------------------------------------------------------


The "Security Functions and Features" section outlines the built-in security measures of your project. This section can be structured as you see fit, but it should include the component name, its applicability, and a description of its importance. This information will be useful for threat modeling later on.

Applicability is categorized as either "Critical" or "Security Relevant". Critical elements are non-configurable design decisions made to enhance the project's security. Security Relevant elements are parts of the project that users can configure to improve the security posture of an implementation.

The Description of Importance is a brief explanation of why this feature is a crucial part of the project's design and why it should be included in the threat model.

In the example below, our project does not support production usage, so there isn't a long list of built-in security features. However, we can use this section to inform our security improvements on the roadmap!

Example-------------------------------------------------------------------------------------

## Security functions and features
| Component | Applicability | Description of Importance |
| --------- | ------------- | ------------------------- |
| Hashicorp Go-Plugin | Critical | The `Go-Plugin` component enables Privateer to segment Raids as fully independent processes that communicate with the core via RPC on a local network, thereby allowing plugins to operate side-by-side without opportunity for configuration collision or side-channeling. |
| YAML Configuration | Relevant | The YAML configuration handling enables Privateer to safely read user configuration and secrets across multiple Raid executions while encrypting or masking them when appropriate |

End of Example------------------------------------------------------------------------------

The "Project Compliance Overview" section is where you indicate whether your project complies with any regulatory standards such as PCI-DSS, COBIT, ISO, GDPR, among others. This information can help streamline the review audit process later on.

If your project is compliant with certain standards, you should list those standards and explain how their compliance has been validated.

If your project is not compliant, it's important to explain why. For instance, the project might not be intended for use in regulated settings, or it might still be in the early stages of development. If the project will need to comply with certain standards in the future, that should also be noted.

In the example below, regulatory compliance may be a consideration in the distant future, but for now, the focus is on creating tools for use in non-production environments. This means compliance is currently out of scope.

Example-------------------------------------------------------------------------------------


## Project Compliance
Privateer does not currently adhere to any compliance standards. This is because the currently supported usage of Privateer is to execute raids on non-production environments.
### Future State
The Privateer roadmap includes preparation for eventual production support, which is why we are seeking to hold Privateer to a high security standard. We hope to include prod support sometime after the official v1 release.

End of Example------------------------------------------------------------------------------

Secure Development Practices:
This section has three values. After providing the summary statement, we’ll nest the other two values beneath subheadings, with narrative explanations for each section.
- Secure development practices overview
- Development pipeline (SDLC, provenance, etc.)
- Communication channels (community, collaboration, etc.)

Development Pipeline:
- Do you have branch protection or repo security features in place?
- Are committers required to sign their commits, or a contributor license agreement?
- Do you have automated testing or fuzzing on every pull request?
- Do you have software composition analysis or dependency management tooling?
- How many reviewers are required for a pull request to be approved?
- Do you have any measures around code owners?
- Is your release process automated?
- Does every release include an automatically generated Software Bill of Materials?
- Do you sign releases?
- Are container images immutable and signed?

Example-------------------------------------------------------------------------------------


### Deployment Pipeline
In order to secure the SDLC from development to deployment, the following measures are in place. Please consult the roadmap for information about how this list is growing.
- Branch protection on the default (`main`) branch:
  - Require signed commits
  - Require a pull request before merging
    - Require approvals: 1
    - Dismiss stale pull request approvals when new commits are pushed
    - Require review from Code Owners
    - Require approval of the most recent reviewable push
    - Require conversation resolution before merging
  - Require status checks to pass before merging
    - Require branches to be up to date before merging

End of Example--------------------------------------------------------------------------------


The "Communication Channels" section provides an overview of how the project team communicates. It includes details about various communication platforms like Slack channels, recurring meetings, mailing lists, and so on. Here's a breakdown:

- **Internal**: This refers to how team members communicate with each other. This could be through internal messaging platforms, email, or regular meetings.

- **Inbound**: This refers to how users or prospective users communicate with the team. This could be through public forums, user feedback forms, social media, or customer support channels.

- **Outbound**: This refers to how the team communicates with its users. This could be through newsletters, blog posts, social media updates, or direct emails.

Example---------------------------------------------------------------------------------------


### Communication Channels
Internal communications among Privateer maintainers and contributors are handled through the public Slack channel and direct messages. Inbound communications are accepted through GitHub Issues or the public slack channel and direct messages. Outbound messages to users are made primarily via documentation or release notes, and secondarily via the public slack channel.

End of Example--------------------------------------------------------------------------------


Security Issue Resolution:
Security issue resolution encompasses both responsible disclosure and incident response.
At the top of this section, give users a quick link to where they can find your official security policy. We will expand on this through the next subsections.

Example---------------------------------------------------------------------------------------


## Security Issue Resolution
The Privateer security policy is maintained in the SECURITY.md file and can be quickly found through the [GitHub Security Overview](htt‌ps://github.com/privateerproj/privateer/security/policy).

End of example--------------------------------------------------------------------------------

 
Responsible Disclosure Practices:
A process through which the user/researcher can disclose findings related to vulnerabilities and weaknesses in your project.
GitHub has a built-in Security tab at the top of the repository page.

Example---------------------------------------------------------------------------------------

### Responsible Disclosure Practice
The Privateer project accepts vulnerability reports through the [GitHub Vulnerability Reporting](htt‌ps://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability) tool.
Anyone can make a report by going to the [reporting form](htt‌ps://github.com/privateerproj/privateer/security/advisories/new) in the GitHub repo. In the event that a report is received, a maintainer will collaborate directly with the reporter through the Security Advisory until it is resolved.

End of example--------------------------------------------------------------------------------


Incident Response:
Use this section to document your project’s process for triage, confirmation, notification of vulnerability or security incident, and patching/update availability.
If your project lacks a comprehensive plan for incident response, then include as much detail as you can—and be sure to include this gap on your project’s roadmap!

Example---------------------------------------------------------------------------------------


### Incident Response
In the event that a vulnerability is reported, the maintainer team will collaborate to determine the validity and criticality of the report. Based on these findings, the fix will be triaged and the maintainer team will work to issue a patch in a timely manner.
Patches will be made to all versions that are currently supported under the project's security policy. Information will be disseminated to the community through all appropriate outbound channels as soon as possible based on the circumstance.

End of example--------------------------------------------------------------------------------



The last section is the appendix. Here are some elements you should include, according to CNCF’s TAG Security:

#### 1. Historical Vulnerabilities
Include a list or summary of past vulnerabilities, complete with links for reference. If there have been no reported vulnerabilities, share any available data on your success rate in identifying issues during code reviews or automated testing.

#### 2. OpenSSF Best Practices
Discuss the current status of the project in relation to OpenSSF best practices. Outline what needs to be done for the project to earn the OpenSSF badge.

#### 3. Case Studies
To provide context for reviewers, detail 2-3 real-world use cases where the project has been implemented.

#### 4. Comparison with Similar Projects
If potential users have previously asked about how your project differs from similar solutions, include a brief explanation here. Reviewers may have the same question, so it's beneficial to address it upfront.













