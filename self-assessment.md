# Self-assessment
The Self-assessment is the initial document for projects to begin thinking about the
security of the project, determining gaps in their security, and preparing any security
documentation for their users. This document is ideal for projects currently in the
CNCF **sandbox** as well as projects that are looking to receive a joint assessment and
currently in CNCF **incubation**.

For a detailed guide with step-by-step discussion and examples, check out the free 
Express Learning course provided by Linux Foundation Training & Certification: 
[Security Assessments for Open Source Projects](https://training.linuxfoundation.org/express-learning/security-self-assessments-for-open-source-projects-lfel1005/).

# Self-assessment outline

## Table of contents

* [Metadata](#metadata)
  * [Security links](#security-links)
* [Overview](#overview)
  * [Actors](#actors)
  * [Actions](#actions)
  * [Background](#background)
  * [Goals](#goals)
  * [Non-goals](#non-goals)
* [Self-assessment use](#self-assessment-use)
* [Security functions and features](#security-functions-and-features)
* [Project compliance](#project-compliance)
* [Secure development practices](#secure-development-practices)
* [Security issue resolution](#security-issue-resolution)
* [Appendix](#appendix)

## Metadata

A table at the top for quick reference information, later used for indexing.

|   |  |
| -- | -- |
| Software | [Operator Framework](https://github.com/operator-framework/operator-sdk)  |
| Security Provider | No  |
| Languages | Go, Ansible, Python, C++ |
| SBOM | [git](https://git-scm.com/downloads), [go.mod](https://github.com/operator-framework/operator-sdk/blob/master/go.mod), [go.sum](https://github.com/operator-framework/operator-sdk/blob/master/go.sum), [Kubernetes](https://github.com/kubernetes/kubernetes), |
| | |

### Security links

Provide the list of links to existing security documentation for the project. You may
use the table below as an example:
| Doc | url |
| -- | -- |
| Security file | https://github.com/operator-framework/operator-sdk/blob/master/SECURITY.md |
| Default and optional configs | https://github.com/operator-framework/operator-sdk/blob/master/config/crd/bases/_.yaml |

## Overview

Operator Framework is an open-source toolkit that provides the runtime environment and software development kit (SDK) for building and running Kubernetes applications, dubbed “Operators”, in an effective and easily scalable way.

The SDK is a framework that allows developers to build and manipulate Operators without prior knowledge of the complexities of Kubernetes API. By providing libraries and tools, developers can use familiar languages to streamline the process.

### Background

Streamlining processes for consumers, workers, and industries in general is often of utmost importance. It saves time, money, and headaches for everyone involved. When it comes to installing a piece of software, most expect it to be simple with only a few clicks involved, the same applies to updates and upgrades for that software. However every machine is different and each application has numerous dependencies that are necessary to run it correctly.To expect consumers or people who are not as proficient with technology to install the correct one from slightly different variations would be nonsensical.

Containerization solves this problem. Containerization is the process of packaging an application with all its dependencies into a single, self-contained unit called a container. This would include the application code, source tools, runtime libraries, and more. By providing an isolated environment, this ensures that the application would be consistent in running on multiple different machines, regardless of the individual conditions.Containerization allows for ease of use, portability, scalability, and efficient deployment of applications.

A developer team seeking to utilize these benefits for their application would find Kubernetes very helpful. Kubernetes is an open source containerization platform that automates application deployment, updates, and overall management. It provides the container infrastructure and allows developers to define their application requirements. These reasons alone with a host of other benefits would be a great help to any development team.

However, setting up and configuring Kubernetes clusters can pose a challenge for developers unfamiliar with Kubernetes. It requires an in-depth understanding of networking, infrastructure, and storage concepts. These are significant learning curves that create a “threshold filter” of sorts that leads developers to deciding on a different alternative to Kubernetes. 

Operator Framework is a solution to this problem along with providing several benefits that Kubernetes does not natively supply. Operator Framework allows developers to build Operators with languages and libraries that they are already familiar with. Operators built with Operator Framework allow for even more options for automation of tasks and workflows beyond the basic functionalities provided by Kubernetes. Overall Operator Framework complements the benefits that Kubernetes offers by providing the specialized tools for easier learning for developers, better automation, and better scalability making Kubernetes an easier and more powerful tool.

### Actors
Operator Framework is comprised of a few different parts including, 

#### Operator Framework SDK
The framework used to build and package Operators. Using Operator SDK allows developers to easily automate and manage any Operators they create. It constructs basic manifests such as the CRD, RBAC, Dockerfile­, and the primary file "main.go", and offers some sample illustrations. The user interacts with the Operator SDK through a command line interface (CLI) to create, test, and build operators and manage the Operator Life Cycle Manager (OLM) installation in the cluster.

#### Operator Lifecycle Manager (OLM) 
Contains two parts, Operator OLM and Catalog Operator. Both provides the runtime environment and APIs for managing the lifecycle of Operators and their resources. It also helps in deploying, installing, and updating Operators. Operator OLM is for manually created Operators and Catalog Operator is for Operators taken from Operater Hub. 

#### Operator Registry
The Operator Registry is essentially a gRPC API that provides the OLM with operator bundle data, allowing querying of these operator bundles. It provides several binaries, including ```opm```(which updates registry databases and the index images), ```initializer```(which takes operator manifests as inputs and outputs SQLite database with the data allowing querying), and many more.

#### Operator Hub
A community-driven public hub for sharing and discovering Operators for various uses.

These components make up Operator Framework and make it very useful for developing, deploying and managing Operators.

### Actions

#### Creating an Operator using Operator Framework SDK
1. Create new project operator using the SDK Command Line Interface (CLI)
2. Define resource APis by adding Custom Resource Definitions
3. Define controllers
4. Write reconciling logic for controllers using SDK and APIs
5. Using the SDK CLI, define webhooks for the custom resource, e.g., validating/mutating webhooks if necessary.
6. Use the SDK Command Line Interface to generate the operator deployment manifest

#### Installing and Managing an Operator using Operator Framework Operator Lifecycle Manage (OLM)
1. Use Operator OLM to manually create Operator
  OR
1. Use Catalog Operator to create Operator from OperaterHub

Operator OLM
1. Watches for the ClusterServiceVersion (CSV) in a namspace and checks to make sure the requirements are met
2. If requirements are met, the install strategy is ran

Catalog Operator
1. Holds a cache of CSVs and CRDs.
2. Watches for InstallPlans set by user
3. If one is found, finds the matching name and adds as a resource, else 7b
4. For each managed CRD, adds as a resolved resource
5. For each resolved CRD, finds the managing CSV
6. Watches for resolved InstallPlans and creates resources for them
7. Watches for subscriptions to Operators in Catalog, and creates InstallPlans for them

### Goals
The goals of Operator Framework are mainly to simplify and enhance applications  on Kubernetes clusters. 

It does this by using the Operator SDK to simplify creation and automation of Operators. This thoroughly increases developer productivity.

Operator Framework also enhances the reliability of complex applications by allowing them to declare specific configurations to make sure the application is always running as desired. This would in turn reduce possible downtime and making it easier for the responsible department

For security guarantees, Operator Framework uses the same principles as Kubernetes such as Role-Based Access Control so that applications cannot act outside of the scope provided. This along with other network isolation policies ensures the security aspect. 

### Non-goals
Although Operator Framework enhances many basic features of Kubernetes, as a result it also shares some of the same non-goals. The logging and monitoring, although provided, are basic as they are not meant to be a replacement for fully comprehensive security logging and monitoring alternatives.

They also do not cover every aspect of deployment of applications. They mainly focus on the lifecycle of the Operator itself. This includes the deployment, upgrading, and scaling of Operators while leaving out other aspects of deployment like storage and networking.

## Self-assessment use

This self-assessment is created by the Security Pals team to perform an internal analysis of the
project's security.  It is not intended to provide a security audit of Security Pals, or
function as an independent assessment or attestation of Security Pals's security health.

This document serves to provide Security Pals users with an initial understanding of
Security Pals's security, where to find existing security documentation, Security Pals plans for
security, and general overview of Security Pals security practices, both for development of
Security Pals as well as security of Security Pals.

This document provides the CNCF TAG-Security with an initial understanding of Security Pals
to assist in a joint-assessment, necessary for projects under incubation.  Taken
together, this document and the joint-assessment serve as a cornerstone for if and when
Security Pals seeks graduation and is preparing for a security audit.

## Security functions and features

### Critical

#### Operator SDK
The user is not allowed to modify the underlying logic for scaffolding, testing and deployment as they define the metadata and other manifests for the operator. It also performs validation checks on the Operator's code, bundle, and catalog, and the user cannot bypass these checks.

#### Operator Lifecycle Manager (OLM)
The user is only allowed to specify the desired state of the operators, and OLM reconciles the actual state and the desired state without any user intervention in the OLM reconciliation logic.

#### Operator Registry 
The user only specifies the Operator bundle and catalog metadata, while the registry automatically generates the necessary manifests and indexes. Manual modification of the registry's manifest generation/indexing logic is not allowed, and it is implemented internally by the OPM tool and the registry server. The registry also performs validation checks on the operator bundle before adding them to the registry so that no error impacts the Operator installation/update, and the user is not allowed to bypass these checks to ensure proper functioning. 


### Security Relevant
* Security Relevant.  A listing of security relevant components of the project with
  brief description.  These are considered important to enhance the overall security of
the project, such as deployment configurations, settings, etc.  These should also be
included in threat modeling.

## Project compliance

Not applicable.

## Secure development practices

### Development Pipeline
All Code is maintained in [Github](https://github.com/operator-framework) and changes are reviewed by maintainers
* The Source Code is visible in the Github
* Changes are submitted through Pull Requests
* Pull Requests automatically have checks performed
* Pull Requests are reviewed by maintainers
* Merges are performed after passing checks and review by maintainer

Operators employ several techniques that ensure their security and integrity
* Operators follow the principle of least privelege, ensuring that they only access the necessary resources with the least amount of permissions.
* Operators use secure communication channels such as SSH between various components to prevent eavesdropping or mishandling of information
* Operators should employ a Role Based Access System to ensure that only the authorized users and services are allowed to perform actions in the Operator's lifecycle
* Operators are expected to use Kubernetes Secrets objects rather than hardcoding for sensitive data.

### Communication Channels

**Internal**
Team Members communicate with each other through [Operator Framework Google Groups](https://groups.google.com/g/operator-framework) and through [Github issues](https://github.com/operator-framework/operator-sdk/issues).

**Inbound**
Users communicate with the project maintainers through [Operator Framework Google Groups](https://groups.google.com/g/operator-framework), [Operator SDK Contributer Google Groups](https://groups.google.com/g/operator-framework-sdk-dev), [Operator OLM Contributer Google Groups](https://groups.google.com/g/operator-framework-olm-dev), through [Github issues](https://github.com/operator-framework/operator-sdk/issues), and the #kubernetes-operators on the Kubernetes Slack.

**Outbound**
Team Members communicate with users through [Operator Framework Google Groups](https://groups.google.com/g/operator-framework).

### Ecosystem
Operator Framework plays an integral part in the Cloud Native Ecosystem. They promote the development and automation of specialized Operators for complex services and applications. They enhance many of Kubernetes basic features and account for the shortcomings as well.

They fullfill a special service of allowing for the development of Operators with familiar languages and libraries. This alone creates a large efficiency boost for the development team, which is increased even moreso when taking into the consideration the aforementioned automation aspects of Operator Framework.

## Security issue resolution

### Reporting a vulnerability
Security Vulnerabilities are handled by the Red Hat Product Security and can be reported by sending a mail to secalert@redhat.com.

The sent email will be read and acknowledged with a non-automated response within three working days. The security team requires several information like steps to reproduce, version number etc. which are available [here](https://access.redhat.com/security/team/contact).
### Creating an issue
Issues can be created at [opening an issue](https://github.com/operator-framework/operator-sdk/issues/new). More information about how to create an issue can be found [here](https://sdk.operatorframework.io/docs/contribution-guidelines/reporting-issues/).

Issues are tracked [here](https://github.com/operator-framework/operator-sdk/issues).
### Issue Lifecycle
#### Triage Meetings
Each week, there is a triage meeting to review new issues. Each issue that has been filed since the previous meeting is discussed, GitHub labels are applied, and the issue is added to a Milestone. Additionally, anyone can request that a previously triaged issue can be retriaged.
#### Grooming 
Following a release, there is a [grooming meeting](https://github.com/operator-framework/community#operator-sdk-grooming-meeting) to review issues that are desired in the next release. Issues are discussed in the following order:
* Issues in the next release milestone
* Issues labeled as priority/important-soon
* Issues in other milestones/backlog if specifically requested

### Operator SDK response team
* Austin Macdonald (**[@asmacdo](https://github.com/asmacdo)**), Red Hat
* Jonathan Berkhahn (**[@jberkhahn](https://github.com/jberkhahn)**), IBM
* Ken Sipe (**[@kensipe](https://github.com/kensipe)**), Code Mentor
* Varsha Prasad Narsing (**[@varshaprasad96](https://github.com/varshaprasad96)**), Red Hat

The specific details about the timings of the meetings and communication channels are available [here](https://github.com/operator-framework/community#operator-sdk-working-group).


## Appendix

### Known Issues Over Time

All reported bugs, issues, and fixes can be viewed from [operator-framework/operator-sdk/issues repository](https://github.com/operator-framework/operator-sdk/issues). However, there is no designated label for all security related issues.

### CII Best Practices

The project has [not been documented](https://www.bestpractices.dev/en/projects) to have achieved the passing level criteria for CII best practices.

### Related Projects

- **Kubebuilder** is an SDK for creating Kubernetes APIs using CRDs and controllers, it is based on the controller-runtime library also used by the Operator SDK. The primary difference is that while the Kubebuilder focuses on building APIs and controllers, the Operator Framework provides additional tools for managing operator lifecycle and distributions. The Github repository is under active maintenance by the community.

- **Metacontroller** is a similar framework that enables users to build custom controllers in any programming language that is supported by the Kubernetes pods. This is because it does not use CRDs or controller-runtime, it rather relies on webhooks and custom resources to communicate with the API server. Similar to Kubebuilder, it does not provide resources for operator lifecycle management and distributions; and, it is not as actively maintained as the Operator Framework.

- **Crossplane** is more of a cloud focused tool, i.e., it manages connecting, consuming and provisioning cloud resources using CRDs and controllers. It also provides a package manager and container registry for the installation and distribution of controllers. The Github repository for Crossplane is under actively maintained.


* Known Issues Over Time. List or summarize statistics of past vulnerabilities
  with links. If none have been reported, provide data, if any, about your track
record in catching issues in code review or automated testing.
* [CII Best Practices](https://www.coreinfrastructure.org/programs/best-practices-program/).
  Best Practices. A brief discussion of where the project is at
  with respect to CII best practices and what it would need to
  achieve the badge.
* Case Studies. Provide context for reviewers by detailing 2-3 scenarios of
  real-world use cases.
* Related Projects / Vendors. Reflect on times prospective users have asked
  about the differences between your project and projectX. Reviewers will have
the same question.
