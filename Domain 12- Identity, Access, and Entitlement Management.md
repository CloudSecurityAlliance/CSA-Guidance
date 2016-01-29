# Domain 12: Identity, Entitlement, and Access Management 

> This domain is in the outline phase, but some pre-existing and new material is more-fully written out.

>Pre-amble: Focus here is not to be prescriptive - but offer a set of checklists and options for each facet of code development and management. Each section will be a short introduction to the concept and the value proposition. This section should be shorter than from previous  original and will cover broader set of subjects. 


## Introduction

This section of the guidance is for Security, Identity and IT teams who want to deploy strong identity systems for SaaS, PaaS and IaaS Cloud environments.

This section covers preparing identity and for use in SaaS, PaaS and IaaS systems. Runtime aspects of identity like authentication, Single Sign On, Multi Factor authentication, and authorization are handled in the Application Security section. In this section, we will cover: 

Guide companies on how to leverage Identity services for SaaS, PaaS and IaaS
Describe the key design decisions that drive identity architecture for SaaS, PaaS and IaaS
Focus on key process and architecture capabilities required for a successful deployment 

This domain focuses on what needs to change in identity management for Cloud. While we review some fundamental concepts, the focus is on how cloud changes Identity Management, and what to do about it

### How Identity management is different in the cloud:

>Focus on the material differences between the in house IT services and application deployment that people are familiar with, and Cloud computing differs. 	Divide into opportunities to make secure better, and new challenges they have not had in the past.

Enterprise identity systems tend to be a patchwork overlaid on disparate technologies and business processes. That’s mainly a result of heterogeneous IT systems where security and identity were layers mainly tacked on afterwards. Cloud applications therefore offer both an opportunity and a challenge to enterprises. 

The opportunity is to deliver a cohesive, unified identity system built in from the beginning. Cloud providers all offer some level of greater capability than previous technology shifts. The challenge comes with how companies choose (if at all) to integrate their existing identity systems with Cloud systems. 

In Cloud, the fundamental problem is multiple organizations are now managing the identity and access management to resources, which can greatly complicate the process. For example, imagine having to provision the same user on dozens, or hundreds, of different cloud services. Federation is the primary tool to manage this problem by building trust relationships between organizations, and enforcing them through standards-based technologies.

Opportunities: 

* Automate Provisioning: There exist various methods to accomplish automated provisioning, but the unifying premise rests on the ability to efficiently, quickly transmit identity information from the authoritative source of identity to Cloud provider systems. These solutions may be process based like provisioning systems or data based like virtual directory or directory integration or application based like APIs.
* Clean Access Rights: User accounts require more than just registration from an authoritative source. The user account must be set up with the appropriate access rights. This requires group, role membership, and attributes to be defined and managed. Many companies spend years reverse engineering roles and access rights for existing users, going to Cloud applications means that they have a chance to do it right the first time.
* Improve Entitlements: This has historically been a real challenge in most IT systems, because permissions are entwined within applications and databases. Because cloud providers are making a product designed for external consumers, these permissions are visible. the net result here is that authorization policies can extend clearly to users through roles and attributes. These roles and attributes then have a simple way to map to resource permissions to make a cohesive access control systems.
* Expand identity monitoring: Due to lessened control in some areas for SaaS, IaaS and PaaS, and due to higher scrutiny for Cloud applications, audit trails matter more not less. The full lifecycle of the identity should be monitored. This includes all of the critical events from initial  account creation and registration through to deprovisioning. 
* Implement Identity Brokers: Companies can leverage a wide range of identity services in the Cloud. Cloud based Strong authentication services enable companies to drive down cost of multi factor authentication. These authenticated sessions can leveraged to scale out strongly authenticated Single Sign On sessions to a wide variety of SaaS, PaaS, and IaaS providers. Companies can take advantage of Cloud providers centralized user management to ease administration and access reviews.  

Challenges:

* Cloud identity Architecture - An island, a spoke or a hub? : On premises identity system success is generally a function of specific business process and/or technical architecture goals. Identity architects must decide whether to make their SaaS, IaaS and PaaS environments an island apart that’s independent of the rest of the company identity systems or whether to tackle how to integrate Cloud identity and company identity. In the latter case, the Cloud identity has mostly been a spoke hanging off the enterprise identity hub. There are some shifts occurring now with identity being primarily defined in the Cloud, and the Cloud can be considered as a hub in some instances. Answering this question is the single biggest identity architecture decision.
* Privileged user management: Watching the watchers is a tricky task at the best of times. The Cloud adds another dimension to this problem due to the lack of visibility lower down in the stack and the role of the metastructure/management plane.
* More-complex chain of responsibility: Access control in a Cloud system is a chain of responsibilities. Each party has to know and execute on what specific part of the process is in their scope - from account management to assigning rights to access reviews and more. That means a comprehensive governance model is necessary to ensure each participant knows what its function is in the identity chain.
 
>A note on Identity Protocols: Identity protocols represent both opportunities and hurdles. Identity protocols do not represent a complete solution by themselves, but they are a means to an end. The essential concepts when choosing an identity protocol are-

* No protocol is a silver bullet that solves all access control problems
* Identity protocols must be analyzed in the context of use case(s), for example Browser based Single Sign On, API keys, or Mobile to Cloud authentication could each lead companies to a different protocol.
* The key operating assumption should be that identity is a perimeter in and of itself, just like a DMZ. So any identity protocol has to be selected and engineered from the standpoint that it can traverse risky territory and withstand malice. 

The most commonly deployed identity protocols are *SAML*, *OAuth*, and *OpenID Connect*. Its critical to identify what your use case proposition and chain of responsibilities are first and then select an identity protocol second.

The most common capability for enterprises is to ensure that access control decisions are based on an authoritative source of identity with the freshest, most specific and accurate information about employees, contractors, developers, customers, and users. This generally means that the information for that purpose is in an enterprise HR and/or directory system. 
 
Beyond the capability to communicate with the authoritative source, there is a common constraint. Namely, to enforce an access control decision the identity protocol must map its identity data to the Cloud Provider’s implementation of permissions and privileges. 

The net result of this desired capability for an authoritative source via enterprise HR and at the same time a terminus in the Cloud authorization is a more or less direct mapping to the problem that federated identity protocols like SAML were designed to solve. 

## Overview

### Cloud Identity Management 

**What is Identity Management?**
Identity management is a set of capabilities that prepares an account for use in the Cloud. That means it exists at the intersection of security and lifecycle management. 

*Identity* is a concept that is simple to understand and hard to execute on. Achieving scale makes automation a critical factor. Manual processes are likely to play a role over the short to medium term especially in access assignment and reviews, but the long run goal should be towards as much automation in the lifecycle as possible. 

**Identity Policies**
Technical identity policies are required to ensure there is detailed guidance for who has access to what resources. These should go well beyond vague statements like “least privilege.” Technical identity policies and towards prescriptive guidelines than can be used to shape detailed process workflows necessary for automating identity flows. We will examine identity design considerations, tools and techniques that enable policies to drive stronger identity architecture.

#### Cloud Identity Management Overview 

This section describes design considerations for identity management capabilities in cloud services.

**User Management**
The User management system is the backbone of identity and therefore the backbone of access control in the system. The Cloud system’s access control can never be stronger than the strength of its identity proofing and user management systems. 

**Authoritative Source(s)**
The User management system should be set up with well understood authoritative source(s) of identity. There are several variations on how this can be handled. 

The most common form of authoritative source is to use the company HR system, extract the relevant parts of that data on a regular basis and feed it to the Cloud system. This model works well at a high level, because it can handle both provisioning new accounts as well as disabling or deprovisioning accounts. However, the HR system will likely lack information to provision customers, partners, consumers, contractors, and social users. So if these constituents are required then additional authoritative sources are needed to close this gap.

Another method is to manage users directly in the Cloud provider system. That means either doing an initial load or manually adding users up front and then using the Cloud provider as the Authoritative Source for identity. This can make the initial deployment simpler and faster, however it can have long term consequences that may prove difficult over time. Once the users are added, the lifecycle management now needs to exist in another locale. The Cloud application owner has to know add, edit, manage, and remove users. So this lacks automation and ends up being a more manual, error prone process. However, if you are starting from scratch with no enterprise backend systems then this can be an effective choice.

The final method is via Federation where the users are not loaded directly into the Cloud system and instead the enterprise Identity Provider (IDP) is used to drive who has access to the system. Assuming the access rights (see Access Management) assignment can work then this can be an effective solution.

**User Lifecycle Management**
The user lifecycle management process is based on the following steps in the process workflow:

>note: turn this into a graphic

* User registration - initial account creation. As noted above this may be done directly in the Cloud provider system or via account synchronization
* Assign access rights - the user account is configured with the set of groups, roles, attributes, or other methods to ensure the user has the proper permissions to access the system
* Assign credentials - the user account must have the right credentials, password, one time password (OTP), multi factor authentication
* Self service account management - there exist a number of important touch points for users to manage their accounts, these include request approval workflows, account access reset and recovery, and profile updates
* Profile management - update account names, rights, privileges and other identity and access control functions
* Deprovisioning - remove or disable access for users
* Audit trail - security and compliance dictate that audit trails must be in place for all critical events

The role of the User Management system is to be the process controller of the user management workflow. The rest of this section with unpack each the above functions in more detail.

**Credential Management**
*Credentials* are what users need to get access to the Cloud system. The most basic form factor is the password. 

*Passwords* have a number of problems associated with them. They are easy to guess, may be easy to steal,  and they are often reused. In general, they should be avoided or used in a limited, controlled fashion.  

Where they are used, passwords policies should follow a number of long standing guidelines to ensure they are strong and complex. To ensure this is the case, the password policy should implement a password meter, minimum lengths, and ensure there are multiple character sets and special characters required.

Cloud applications are always on. This makes them highly vulnerable to guessing attacks and brute force. The system must have strict policies to recognize and limit retries including low and slow retries. Further, the system should try to recognize when an account is compromised by analyzing its behavior.

Cloud systems should look beyond passwords to further strengthen authentication.

**Strong Authentication**
There is a range of options for strong authentication beyond passwords. The main determine factor for success here will be the intersection of security and usability. There are a number of ways to get higher assurance than a password, however its a much shorter list to do so at a reasonable cost with minimal friction to the user base. 

*Hard Token based authentication* has the advantage of strong one time passwords. Soft token can make the usability simpler by removing the requirement for the physical hard token. 

Techniques such as *SMS and Mobile form factors* may offer a good combination through the separate physical device (Smartphone) without the overhead associated with tracking and managing the hard token. 

For OTP tokens consider OATH as the starting point for token generation. 

For customers, FIDO is one standard that may streamline stronger authentication for consumers while minimizing friction.

### Access Management

Access management determines what someone has access to, based on their identity.

Access management is an area where companies moving to the Cloud have a good opportunity to improve upon current state practice in the enterprise. The reason is that most Cloud services have well defined interfaces whereas many enterprise systems lack a well defined registry of resources, and services. 

**Group and role management**
Each user account should be assigned to a set of groups, roles, and attributes that drive its access rights. Its important to remember that the role or group by itself is not the access control scheme. Instead its the combination of the group/role/attribute when mapped to the permissions/privileges of the service. Both of these are required for a cohesive system. Roles and groups are primarily an administrative convenience, a means to an end but not an end in and of themselves.

Roles may be based on technical roles, business roles or a mix. Business roles have the advantage of mapping to business processes but they lack the technical context needed to make runtime access control decisions. Technical roles have technical fidelity to the system, but they are near impossible for managers to approve because they map to IT systems view of the world. Role engineering here is about right sizing the access between the business high level view and the low level technical views. 

**Entitlements**
An *entitlement* is the expression of an access right. what identity has what privilege based on what attributes.

The permissions are based on the service or resource that the user needs to access. The permissions can be fairly simple - Create, Read, Update, Delete for example. However, the role engineering process means that the numerous objects and permissions must be distilled down to a very small set of roles. This can be a time consuming, but important task.

An *entitlement matrix* is a document/chart that defines the various entitlements, which are then enforced via policies and controls.

**Enforcement**
Policy decision points and policy enforcement points. Policies are typically managed by the cloud customer, but are enforced by the cloud provider.


### Privileged User Management

In terms of controlling downside risk, few things are more essential than privileged user management. The requirements mentioned above for strong authentication should be a strong consideration for any privileged user. In addition, account and session recoding should be implemented to drive up accountability and visibility for privileged users.

In some cases, it will be beneficial for Privileged user to sign through a separate tightly controlled system using higher levels of assurances for credential control, digital certificates, physically and logically separate access points, and/or jump hosts

### User Activity Monitoring
Access control by itself is not sufficient to cope with threats to Cloud systems. Accountability and visibility is required to limit the potential for malicious use. 

Log sensors should be in place to record an auditable trail for all user activity. The goal is to provide forensic capabilities, give the IRT team what it needs to respond to an event, and to limit an attacker’s freedom of movement in the system.


## Recommendations

* Automate provisioning of user accounts based on an authoritative source
* Tie provisioning to events like start dates, termination events, reassignment, bulk reassignment
* Remember “other” users - developers, contractors
* Assign ownership for assigning Cloud service permissions and privileges
* Turn on extended logging and monitoring to give ongoing visibility into the identity management lifecycle
* Consider use of identity brokers where appropriate
* Develop an entitlement matrix for each cloud provider and project, with an emphasis on access to the metastructure and/or management plane.
* There are no magic protocols, pick your use cases and constraints first and then find the right protocol second
* Ensure you have a plan to deliver high levels of assurance for privileged users
* Identity is the new DMZ. Make sure that you pay proper attention to all aspects of the identity lifecycle
* Harden the identity ecosystem with positive and negative test cases for all critical events 