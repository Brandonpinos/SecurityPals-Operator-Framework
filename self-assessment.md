# Operator-SDK Security Self-Assessment

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
| Software | [https://github.com/operator-framework/operator-sdk](https://github.com/operator-framework/operator-sdk)  |
| Website | [https://sdk.operatorframework.io/](https://sdk.operatorframework.io/) |
| Security Provider | No. The main goal is to bootstrap the creation of custom operators, not perform the role of a security provider.  |
| Languages | Go, Ansible, Helm |
| SBOM | < *Software bill of materials.  Link to the libraries, packages, versions used by the project, may also include direct dependencies.* > |
| | |

### Security links

Provide the list of links to existing security documentation for the project. You may
use the table below as an example:
| Doc | url |
| -- | -- |
| Security file | [https://github.com/operator-framework/operator-sdk/blob/master/SECURITY.md](https://github.com/operator-framework/operator-sdk/blob/master/SECURITY.md) |
| Default and optional configs | ... |

## Overview

One or two sentences describing the project -- something memorable and accurate
that distinguishes your project to quickly orient readers who may be assessing
multiple projects.

### Background

Provide information for reviewers who may not be familiar with your project's
domain or problem area.

### Actors

- #### Operator
An Operator runs as a pod within a Kubernetes cluster and reconciles the actual state of a stateful application with the desired state. It retains predefined methods on how to deploy that particular application, how to create multiple replicas of said operation (scaling) and how to recover in the instance of data losses/pod shutdown etc. Tasks are automated and reusable. Essentially, operators are custom controllers for stateful applications.

- #### Operator SDK
The Ope­rator SDK serves as a toolkit, offering a structure­ and libraries that streamline the­ crafting of custom Kubernetes ope­rators and minimize repetitive­ code. It constructs basic manifests such as the CRD, RBAC, Dockerfile­, and the primary file "main.go", and offers some sample illustrations. The user interacts with the Operator SDK through a command line interface (CLI) to create, test, and build operators and manage the Operator Life Cycle Manager (OLM) installation in the cluster.

- #### Operator Lifecycle Manager (OLM)
The Ope­rator Lifecycle Manager ope­rates as a pod inside the Kube­rnetes cluster. It extends the­ API by setting guidelines to install, update­, and manage operators and relate­d dependencie­s. The Operator Framework divide­s the management of life­cycle tasks from the operators, making de­velopment easie­r. This approach offers strong modular design.


- #### Operator Registry
The Operator Registry is essentially a gRPC API that provides the OLM with operator bundle data, allowing querying of these operator bundles. It provides several binaries, including ```opm```(which updates registry databases and the index images), ```initializer```(which takes operator manifests as inputs and outputs SQLite database with the data allowing querying), and many more.

- #### OperatorHub
A website that allows users to browse, search and install relevant operators that are made publicly available.





### Actions
These are the steps that a project performs in order to provide some service
or functionality.  These steps are performed by different actors in the system.
Note, that an action need not be overly descriptive at the function call level.  
It is sufficient to focus on the security checks performed, use of sensitive 
data, and interactions between actors to perform an action.  

For example, the access server receives the client request, checks the format, 
validates that the request corresponds to a file the client is authorized to 
access, and then returns a token to the client.  The client then transmits that 
token to the file server, which, after confirming its validity, returns the file.

### Goals
The intended goals of the projects including the security guarantees the project
 is meant to provide (e.g., Flibble only allows parties with an authorization
key to change data it stores).

### Non-goals
Non-goals that a reasonable reader of the project’s literature could believe may
be in scope (e.g., Flibble does not intend to stop a party with a key from storing
an arbitrarily large amount of data, possibly incurring financial cost or overwhelming
 the servers)

## Self-assessment use

This self-assessment is created by the [project] team to perform an internal analysis of the
project's security.  It is not intended to provide a security audit of [project], or
function as an independent assessment or attestation of [project]'s security health.

This document serves to provide [project] users with an initial understanding of
[project]'s security, where to find existing security documentation, [project] plans for
security, and general overview of [project] security practices, both for development of
[project] as well as security of [project].

This document provides the CNCF TAG-Security with an initial understanding of [project]
to assist in a joint-assessment, necessary for projects under incubation.  Taken
together, this document and the joint-assessment serve as a cornerstone for if and when
[project] seeks graduation and is preparing for a security audit.

## Security functions and features

* Critical.  A listing critical security components of the project with a brief
description of their importance.  It is recommended these be used for threat modeling.
These are considered critical design elements that make the product itself secure and
are not configurable.  Projects are encouraged to track these as primary impact items
for changes to the project.
* Security Relevant.  A listing of security relevant components of the project with
  brief description.  These are considered important to enhance the overall security of
the project, such as deployment configurations, settings, etc.  These should also be
included in threat modeling.

## Project compliance

* Compliance.  List any security standards or sub-sections the project is
  already documented as meeting (PCI-DSS, COBIT, ISO, GDPR, etc.).

## Secure development practices

* Development Pipeline.  A description of the testing and assessment processes that
  the software undergoes as it is developed and built. Be sure to include specific
information such as if contributors are required to sign commits, if any container
images immutable and signed, how many reviewers before merging, any automated checks for
vulnerabilities, etc.
* Communication Channels. Reference where you document how to reach your team or
  describe in corresponding section.
  * Internal. How do team members communicate with each other?
  * Inbound. How do users or prospective users communicate with the team?
  * Outbound. How do you communicate with your users? (e.g. flibble-announce@
    mailing list)
* Ecosystem. How does your software fit into the cloud native ecosystem?  (e.g.
  Flibber is integrated with both Flocker and Noodles which covers
virtualization for 80% of cloud users. So, our small number of "users" actually
represents very wide usage across the ecosystem since every virtual instance uses
Flibber encryption by default.)

## Security issue resolution

* Responsible Disclosures Process. A outline of the project's responsible
  disclosures process should suspected security issues, incidents, or
vulnerabilities be discovered both external and internal to the project. The
outline should discuss communication methods/strategies.
  * Vulnerability Response Process. Who is responsible for responding to a
    report. What is the reporting process? How would you respond?
* Incident Response. A description of the defined procedures for triage,
  confirmation, notification of vulnerability or security incident, and
patching/update availability.

## Appendix

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
