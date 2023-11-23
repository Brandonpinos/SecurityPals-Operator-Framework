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









