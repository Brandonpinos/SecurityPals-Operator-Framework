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

- #### Building an Operator
```operator-sdk create``` allows users to either generate the scaffold of a Kubernetes API–allowing users to generate basic manifests required to define a new API–or a webhook for an API resource–allowing users to handle HTTP requests following specified events. The necessary manifests include the CRD, the RBAC rules, and operator deployment. The Operator SDK validate's the operator's service account against the RBAC manifest to determine whether the operator has the requisite permissions. The user implements the reconciliation logic for the control loop in this phase that will dictate how the operator handles the conflict between desired and actual state of the custom resource. Once the operator image is built, the user can also use the CLI to push it to a container registry, so that it can be available for deployment on a Kubernetes cluster whenever necessary.

- #### Running an operator
This step could involve either running the operator locally for testing purposes or the actual deployment to a cluster. The CLI/Kubectl allows the user to apply the manifests generated and modified in the previous step, creating the service account, role, role binding and deployment of the operator in the cluster.

If the user wishes to run the operator locally, they can use the CLI to connect to a cluster and run the operator on their machine. Either of these would result in the operator pod registering a controller for the custom resource and watch for events. This step involves reading and writing to the Kubernetes API to manage it's custom resources, and therefore could potentially be vulnerable if not implemented correctly. The Kubernetes API server validates the service account's permission against the role bindings before allowing the operator to manipulate the cluster for added security.

- #### Packaging an operator
Packaging is essentially bunding the operator's metadata, manifests and dependencies. The bundle includes:
  - The metadata with information such as the name, description, maintainer, capabilities, dependencies and so on.
  - CRD, CSV and all the other manifests required to run the operator, for example the RBAC file, webhooks, etc.
  - A bundle.Dockerfile that specifies how to build the bundle image.

The user can use the CLI to generate the bundle directory, validate it's contents and build the bundle image. The validation involves checking the CRDs, CSV, and the other manifests, ensuring that the components adhere to the required specifications. The Operator SDK does not handle encryptions or secure connections during the packaging and pushing the bundle image onto the container registry, as it is typically handled by the registry itself.

- #### Operator Distribution
Distributing the bundle represents publishing an operator bundle to a catalog, which acts like a library that stores different versions of various operators grouped by channels. Each channel represents a stream of updates for a given operator, and the update graph defines the upgrade path between the given versions of an operator within a channel. Using the Operator SDK CLI or the opm tool, the user can create a catalog, add or remove bundles, or manipulate channels and update graphs. Once the catalog is ready, the user can push the catalog image to an operator registry, making it available to the OLM through a CatalogSource object. Unauthorized or improper manipulation of the operator bundle data in this phase could harm the integrity and availability of the operators within the catalog.


- #### Operator Installation
This action requires the following steps:
  - Creating a Subscription object in the OLM, referencing a CatalogSource object and a specified channel, i.e., querying for the catalog image. 
  - OLM will then download the updated operator bundle matching the specified channel in the query.
  - OLM also resolves and installs any requisite dependency for the operator, including CRDs, APIs and even other operators.
  - An OperatorGroup object then is created by the OLM that defines the namespaces and service accounts for the operator, ensuring access validation.
  - Finally, a ClusterServiceVersion (CSV) object is created by the OLM representing the operator's installation and status, i.e., the bundle information and operator's phase, message, reason, and conditions.

The sensitive data exchange happens while reading the bundle data from the registry and writing to the Kubernetes API.



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
