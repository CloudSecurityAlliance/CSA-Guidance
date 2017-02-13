# Domain 5: Information Governance

## Introduction

The primary goal of information security is to protect the fundamental data that powers our systems and applications. As companies transition to cloud computing, the traditional methods of securing data are challenged by cloud-based architectures.  Elasticity, multi-tenancy, new physical and logical architectures, and abstracted controls require new data security strategies. In many cloud deployments, users even transfer data to external — or even public — environments in ways that would have been unthinkable only a few years ago. 

Managing information in the era of cloud computing is a daunting challenge that affects all organizations and requires not merely new technical protections but new approaches to fundamental governance. Although cloud computing has at least some affect on all areas of information governance, it particularly impacts compliance, privacy, and corporate policies due to the increased complexity in working with third parties and managing jurisdictional boundaries.

Definition of information/data governance:

*Ensuring use of data and information complies with organizational policies, standards and strategy — including regulatory, contractual, and business objectives.*
 
Our data is always subject to a range of requirements. Some pushed onto us by others — like regulatory agencies or customers and partners — others that are self-defined based on our risk tolerance or simply how we want to manage operations. Information governance includes the corporate structures and controls we use to ensure we handle data in accordance with our goals and requirements. 

There are numerous aspects of having data stored in the cloud that have an impact on information and data governance requirements. 

* *Multi-tenancy:* Multi-tenancy presents complicated security implications. When data is stored in the public cloud, it’s stored on shared infrastructure with other, untrusted tenants. Even in a private cloud environment it is stored and managed on infrastructure that’s shared across different business units, which likely have different governance needs.

* *Shared security responsibility:* With greater sharing of environments comes greater shared security responsibilities. Data is now more likely to be owned and managed by different teams or even organizations. So, it’s important to recognize the difference between data custodianship and data ownership

	* *Ownership*, as the name says, is about who owns the data. It’s not always perfectly clear. If a customer provides you data, you might own it or they might still legally own it, depending on law, contracts, and policies. If you host your data on a public cloud provider you *should* own it, but that might again depend on contracts.

	* *Custodianship* refers to who is *managing the data*. If a customer gives you their personal information and you don't have the rights to own it, you are merely the custodian. That means you can only use it in approved ways. If you use a public cloud provider they likewise become the custodian of the data, although you likely *also* have custodial responsibility depending on what controls you implement and manage yourself. Using a provider doesn’t obviate your responsibility. Basically, the owner defines the rules (sometimes indirectly through regulation) and the custodian implements the rules. The lines and roles between owner and custodian are impacted by cloud infrastructure, particularly in the case of public cloud.

	By hosting customer data in the cloud, we are introducing a third party into the governance model, the cloud provider.

* *Jurisdictional boundaries and data sovereignty* - Since cloud, by definition, enables broad network access, it increases the opportunities to host data in more locations (jurisdictions) and reduced the friction in migrating data. Some providers may not be as transparent about the physical location of the data, while in other cases additional controls may be needed to restrict data to particular locations.

* *Compliance, regulations, and privacy policies* -  All of these may be impacted by cloud due to the combination of a 3rd party provider and jurisdictional changes. E.g. your customer agreement may not allow you to share/use data on a cloud provider, or may have certain security requirements (like encryption).

* *Destruction and removal of data* - This ties in to the technical capabilities of the cloud platform. Can you ensure the destruction and removal of data in accordance with policy?

When migrating to cloud, use it as an opportunity to revisit information architectures. Many of our information architectures today are quite fractured as they were implemented over sometimes decades in the face of ever-changing technologies. Moving to cloud creates a green field opportunity to reexamine how you manage information and find ways to improve things. Don't lift and shift existing problems.

## Overview

*Data/information governance* means ensuring that the use use of data and information complies with organizational policies, standards, and strategy. This includes regulatory, contractual, and business requirements and objectives. Data is different than information, but we tend to use them interchangeably. Information is data with value. For our purposes, we use both terms to mean the same thing since that is so common.

### Cloud Information Governance Domains

We will not cover all of data governance, but we’ll focus on where hosting in the cloud affects data governance. Cloud computing affects most data governance domains:

* *Information Classification.* This is frequently tied to compliance and affects cloud destinations and handling requirements. Not everyone necessarily has a data classification program, but if you do you need to adjust it for cloud computing. 

* *Information Management Policies.*  This ties to classification and the cloud needs to be added if you have them. It should also cover the different SPI tiers, since sending data to a SaaS vendor versus building your own IaaS app is very different. You need to determine, what is allowed to go where in the cloud? Which products and services? With what security requirements? 

* *Location and Jurisdiction Polices.* These have very direct cloud implications. Any outside hosting must comply with locational and jurisdictional requirements. Understand that internal policies can be changed for cloud computing, but legal requirements are hard lines. (See the legal domain for more information on this.) Make sure you understand that treaties and laws may create conflicts. You need to work with your legal department when handling regulated data to ensure you comply as best you can.

* *Authorizations.* Cloud computing requires minimal changes to authorizations, but see the data security lifecycle to understand if the cloud impacts.

* *Ownership.* Your organization is always responsible for data and information and that can't abrogated when moving to the cloud.

* *Custodianship.* Your cloud provider may become custodian. Data hosted but properly encrypted is still under custodianship of the organization.

* *Privacy.* Privacy is a sum of regulatory requirements, contractual obligations, and commitments to customers (e.g. public statements). You need to understand the total requirements and ensure information management and security policies align.

* *Contractual controls.* This is your legal tool for extending governance requirements to a third party, like a cloud provider.

* *Security controls.* Security controls are the tool to implement data governance. They change significantly in cloud computing. See the Data Security and Encryption domain.

### The Data Security Lifecycle

Although Information Lifecycle Management is a fairly mature field, it doesn’t map well to the needs of security professionals. The Data Security Lifecycle is different from Information Lifecycle Management, reflecting the different needs of the security audience. This is a summary of the lifecycle, and a complete version is available at http://www.securosis.com/blog/data-security-lifecycle-2.0 It is simply a tool to help understand the security boundaries and controls around data. It’s not meant to be used as a rigorous tool for all types of data. It's a modeling tool to help evaluate data security at a high level and find focus points.

The lifecycle includes six phases from creation to destruction.  Although it is shown as a linear progression, once created, data can bounce between phases without restriction, and may not pass through all stages (for example, not all data is eventually destroyed).

1. Create. Creation is the generation of new digital content, or the alteration/updating/modifying of existing content.
2. Store. Storing is the act committing the digital data to some sort of storage repository and typically occurs nearly simultaneously with creation.
3. Use. Data is viewed, processed, or otherwise used in some sort of activity, not including modification.
4. Share. Information is made accessible to others, such as between users, to customers, and to partners.
5. Archive. Data leaves active use and enters long-term storage.
6. Destroy. Data is permanently destroyed using physical or digital means (e.g., cryptoshredding).

#### Locations and Entitlements

The lifecycle represents the phases information passes through but doesn’t address its location or how it is accessed. 
 
*Locations*

This can be illustrated by thinking of the lifecycle not as a single, linear operation, but as a series of smaller lifecycles running in different operating environments.  At nearly any phase data can move into, out of, and between these environments.

Due to all the potential regulatory, contractual, and other jurisdictional issues it is extremely important to understand both the logical and physical locations of data.

*Entitlements*

When users know where the data lives and how it moves, they need to know who is accessing it and how.  There are two factors here:

1.	Who accesses the data?
2.	How can they access it (device & channel)?

Data today is accessed using a variety of different devices.  These devices have different security characteristics and may use different applications or clients.

#### Functions, Actors, and Controls

The next step identifies the functions that can be performed with the data, by a given actor (person or system) and a particular location.

*Functions*

There are three things we can do with a given datum:

* Read.  View/read the data, including creating, copying, file transfers, dissemination, and other exchanges of information.
* Process. Perform a transaction on the data: update it; use it in a business processing transaction, etc.
* Store.  Hold the data (in a file, database, etc.).

The table below shows which functions map to which phases of the lifecycle:

Table 1—Information Lifecycle Phases

|       | Create | Store | Use | Share | Archive | Destroy |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Read    |  X   |   X   |   X   |   X  |   X   |   X   |
| Process |  X  |       |   X   |      |       |       |
| Store   |     |   X   |       |      |   X   |       |

An actor (person, application, or system/process, as opposed to the access device) performs each function in a location.

*Controls*

A control restricts a list of possible actions down to allowed actions. The table below shows one way to list the possibilities, which the user then maps to controls.

Table 2—Possible and Allowed Controls

|    Function    |     Actor     |   Location   |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Possible | Allowed | Possible | Allowed | Possible | Allowed |
|          |         |          |         |          |         |
|          |         |          |         |          |         |
|          |         |          |         |          |         |


## Recommendations

* Determine your governance requirements for information before planning a transition to cloud. This includes legal and regulatory requirements, contractual obligations and other corporate policies. Your corporate policies and standards may need to be updated to allow a third party to handle data.
* Ensure information governance policies and practices extend to the cloud. This will be done through contractual and security controls.
* When needed, use the data security lifecycle to help model data handling and controls.
* Instead of lifting and shifting existing information architectures take the opportunity of the migration to the cloud to re-think and re-structure what is often the fractured approach used in existing infrastructure. Don’t bring bad habits.