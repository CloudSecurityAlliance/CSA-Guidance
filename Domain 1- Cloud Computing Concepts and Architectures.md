# Domain 1: Cloud Computing Concepts and Architectures

## 1.0 Introduction

This domain provides the conceptual framework for the rest of the Cloud Security Alliance’s guidance.  It describes and defines cloud computing, sets our baseline terminology, and details the overall logical and architectural frameworks used in the rest of the document. 

There are many different ways of viewing cloud computing. It is a technology, a collection of technologies, an operational model, a business model, and more. It is, at its essence, *transformative* and *disruptive*.  It is also moving very, very quickly, and shows no signs of slowing down. While the reference models we included in the first version of this Guidance are still relatively accurate, they are most certainly no longer complete. And even this update can't possibly account for every possible evolution in the coming years.

Cloud computing offers tremendous potential *agility*, *resiliency*, and *economic* benefits. Organizations can move faster (since they don't have to purchase and provision hardware, and everything is software defined), reduce downtime (thanks to inherent elasticity and other cloud characteristics), and save money (due to reduced capital expenses and better demand and capacity matching). We also see *security* benefits since cloud providers have significant economic incentives to protect customers.

However, these benefits only appear if you understand and adopt *cloud native* models and adjust your architectures and controls to align with the features and capabilities of cloud platforms. In fact, taking an existing application or asset and simply moving it to a cloud provider without change will often reduce agility, resiliency, and even security, all while increasing costs. 

The goal of this domain is to build the foundation that the rest of the document, and the recommendations, are based on. To provide a common language and understanding of cloud computing for security professionals, and to begin highlighting the differences between cloud and traditional computing to help guide security professionals towards adopting cloud native approaches that result in security (and other) benefits, instead of creating more risks.

This domain includes 4 sections:

* Defining cloud computing
* The cloud logical model
* Cloud conceptual, architectural, and reference model
* Cloud security and compliance scope, responsibilities, and models

The Cloud Security Alliance isn't setting out to create an entirely new taxonomy or reference model. Our objective is to distill and harmonize existing models, most notably the work in [NIST Special Publication 800-145][1] and [ISO/IEC 17788/9][2], and focus on what's most relevant to security professionals.

[1]: http://csrc.nist.gov/publications/nistpubs/800-145/SP800-145.pdf
[2]: http://www.iso.org/iso/catalogue_detail?csnumber=60544

## 1.1 Overview

### 1.1.1 Defining Cloud Computing

Cloud computing is a new operational model and set of technologies for managing shared pools of computing resources. 

It is a disruptive technology that has the potential to enhance collaboration, agility, scaling, and availability, and provides the opportunities for cost reduction through optimized and efficient computing.  The cloud model envisages a world where components can be rapidly orchestrated, provisioned, implemented and decommissioned, and scaled up or down to provide an on-demand utility-like model of allocation and consumption.

NIST defines cloud computing as:

>Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction.

The ISO/IEC definition is very similar:

>Paradigm for enabling network access to a scalable and elastic pool of shareable physical or virtual resources with self-service provisioning and administration on-demand.

A (slightly) simpler way of describing cloud is that it takes a set of resources, such as processors and memory,  and puts them into a big pool (in this case, using virtualization). Consumers ask for what they need out of the pool, such as 8 CPUs and 16 GB of memory, and the cloud assigns those resources to the client, who then connects to them over the network and uses them. When the client is done, they can release the resources back into the pool for someone else to use.

A cloud can consist of nearly any computing resources — from our "compute" examples of processors and memory, to networks, storage, and higher level resources like databases and applications. For example, subscribing to a customer relations management application for 500 employees on a service shared by hundreds of other organizations is just as much cloud computing as launching 100 remote servers on a compute cloud.

>Definition: a *cloud consumer* is the person or organization requesting and using the resources, and the *cloud provider* is the person or organization who delivers it. We also sometimes use the terms *client* and *consumer* to refer to the cloud consumer, and *service* or simply *"cloud"* to describe the provider. [NIST 500-292][3] uses the term "cloud actor" and adds roles for cloud brokers, carriers, and auditors.

[3]: http://www.nist.gov/customcf/get_pdf.cfm?pub_id=909505

The key techniques to create a cloud are *abstraction* and *orchestration*. We abstract the resources from the underlying physical infrastructure to create our pools, and use orchestration (and automation) to coordinate carving out and delivering a set of resources from the pools to the consumers. As you will see, these two techniques create all the emergent characteristics we use to define something as a "cloud". 

This is the difference between cloud computing and traditional virtualization; virtualization abstracts resources, but it typically lacks the orchestration to pool them together and deliver them to customers on demand, instead relying on manual processes.

Clouds are *multitenant* by nature. Multiple different consumer consistencies share the same pool of resources but are *segregated* and *isolated* from each other. Segregation allows the cloud provider to divvy up resources to the different groups, and isolation ensures they can't see or modify each others' assets. Multitenancy doesn't only apply across different organizations, it is also used to divvy up resources between different units in a single business or organization.

### 1.1.2 Definitional Model

The Cloud Security Alliance uses the [NIST model for cloud computing][4] as our standard for defining cloud computing. The CSA also endorses the [ISO/IEC model][5] which is more in-depth, and also serves as a reference model. Throughout this domain we will  reference both.

[4]: http://csrc.nist.gov/publications/nistpubs/800-145/SP800-145.pdf
[5]: http://www.iso.org/iso/catalogue_detail?csnumber=60544

NIST’s publication is generally well accepted, and the Guidance aligns with the NIST Working Definition of Cloud Computing (NIST 800-145) to bring coherence and consensus around a common language to focus on use cases rather than semantic nuances.

>It is important to note that this guide is intended to be broadly usable and applicable to organizations globally.  While NIST is a U.S. government organization, the selection of this reference model should not be interpreted to suggest the exclusion of other points of view or geographies. 

NIST defines cloud computing by describing five essential characteristics, three cloud service models, and four cloud deployment models. They are summarized in visual form in Figure 1 and explained in detail below. 

![NIST Model for Cloud Computing](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.1.2-1.png?raw=true)

#### 1.1.2.1 Essential Characteristics

These are the characteristics that make a cloud a cloud. If something has these characteristics, we consider it cloud computing. If it lacks any of them, it is likely not a cloud.

* *Resource pooling* is the most fundamental characteristic, as discussed above. The provider abstracts resources and collects them into a pool, portions of which can be allocated to different consumers (typically based on policies).
* Consumers provision the resources from the pool using *on-demand self service*. They manage their resources themselves, without having to talk to a human administrator.
* *Broad network access* means all resources are available over the network, without any need for direct physical access.
* *Rapid elasticity* allows consumers to expand or contract the resources they use from the pool (provisioning and deprovisioning), often completely automatically. This allows them to more closely match resource consumption with demand (for example, adding virtual servers as demand increases, then shutting them down when demand drops).
* *Measured service* meters what is provided, to ensure consumers only use what they are allotted, and to, if needed, charge them for it. This is where the term *utility computing* comes from, since computing resources can now be consumed like water and electricity, with the client only paying for what they use.

ISO/IEC 17788 lists six key characteristics, the first five of which are identical to the NIST characteristics. The only addition is *multi-tenancy* as separate from resource pooling.

#### 1.1.2.2 Service Models

NIST defines three *service models* which describe the different foundational categories of cloud services:

* *Software as  Service (SaaS)* is a full application managed and hosted by the provider. Consumers access it with a web browser, mobile app, or a lightweight client app.
* *Platform as a Service (PaaS)* abstracts and provides development or application platforms, such as databases, application platforms (e.g. a place to run Python, PHP, or other code), file storage and collaboration, or even proprietary application processing (such as machine learning, big data processing, or direct API access to features of a full SaaS application). The key differentiator is that, with PaaS, you don't manage the underlying servers, networks, or other infrastructure.
* *Infrastructure as a Service (IaaS)* offers access to a resource pool of fundamental computing infrastructure, such as compute, network, or storage. 

We sometimes call this the "SPI" tiers. 

ISO/IEC uses a slightly more complex definition with a *cloud capabilities type* that maps closely to the SPI tiers (application, infrastructure, and platform capability types). It then expands into *cloud service categories* that are more-granular, such as Compute as a Service, Data Storage as a Service, and then even includes IaaS/PaaS/SaaS. 

Some cloud services span these tiers, or don't neatly fall into a single service model. Practically speaking, there's no reason to try and assign everything into these three broad categories, or even the more granular categories in the ISO/IEC model. This is merely a useful descriptive tool, not a rigid framework. 

Both approaches are equally valid, but since the NIST model is more concise and currently used more broadly, it is the definition predominantly used in CSA research. 

#### 1.1.2.3 Deployment Models

Both NIST and ISO/IEC use the same four cloud deployment models. These are how the technologies are deployed and consumed, and apply across the entire range of service models:

* *Public Cloud*.  The cloud infrastructure is made available to the general public or a large industry group and is owned by an organization selling cloud services. 
* *Private Cloud*.  The cloud infrastructure is operated solely for a single organization.  It may be managed by the organization or by a third party and may be located on-premise or off-premise. 
* *Community Cloud*.  The cloud infrastructure is shared by several organizations and supports a specific community that has shared concerns (e.g., mission, security requirements, policy, or compliance considerations).  It may be managed by the organizations or by a third party and may be located on-premise or off-premise.e
* *Hybrid Cloud*.  The cloud infrastructure is a composition of two or more clouds (private, community, or public) that remain unique entities but are bound together by standardized or proprietary technology that enables data and application portability (e.g., cloud bursting for load-balancing between clouds). Hybrid is also commonly used to describe a non-cloud data center bridged directly to a cloud provider.

Deployment models are defined based on the *cloud consumer*; who uses the cloud. As the diagram below shows, the organization that owns and manages the cloud will vary even within a single deployment model.

![Cloud Computing Deployment Models](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.1.2.3-1.png?raw=true)

### 1.1.3 Reference and Architecture Models

These days there is a wide range of technological techniques for building cloud services, making any single reference or architectural model obsolete from the start. The objective of this section is to provide some fundamentals to help security professionals make informed decisions, and provide a baseline to understand more-complex and emerging models. For an in-depth reference architectural model, we again recommend [ISO/IEC 17789][6], and [NIST 500-292][7], which complements the NIST definition model.

[6]: http://www.iso.org/iso/catalogue_detail?csnumber=60544
[7]: http://www.nist.gov/customcf/get_pdf.cfm?pub_id=909505

One way of looking at cloud computing is as a stack where Software as a Service is built on Platform as a Service, which is built on Infrastructure as a Service. This is not representative of all (or even most) real-world deployments, but serves as a useful reference to start the discussion.

![Cloud Reference Architecture](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.1.3-1.png?raw=true)

#### 1.1.3.1 Infrastructure as a Service

Physical facilities and infrastructure hardware forms the foundation of **IaaS**. With cloud computing we abstract and pool these resources, but we always need physical hardware, networks, and storage to build on.  These resources are pooled using abstraction and orchestration. Abstraction, often via virtualization, frees the resources from their physical constraints to enable pooling. Then a set of core connectivity and delivery tools (orchestration) ties these abstracted resources together, creates the pools, and provides the automation to deliver them to customers.

All this is facilitated using *Application Programming Interfaces (APIs)*. APIs are typically the underlying communications method for components within a cloud, some of which (or a different set of) are exposed to the cloud consumer to manage their resources and configurations. Most cloud APIs these days are *REST* based (Representational State Transfer), which runs over the HTTP protocol, making it extremely well suited for Internet services.

In most cases, those APIs are both remotely accessible and wrapped into a web-based user interface. This combination is the *cloud management plane* since consumers use it to manage and configure the cloud resources, such as launching virtual machines (instances) or configuring virtual networks. From a security perspective, it is the both the biggest difference from protecting physical infrastructure (since you can't rely on physical access as a control), and the top priority when designing a cloud security program. If an attacker gets into your management plane, they potentially have full, remote access to your entire cloud deployment.

Thus IaaS consists of a facility, hardware, an abstraction layer, an orchestration (core connectivity and delivery) layer to tie together the abstracted resources, and APIs to remotely manage the resources and deliver them to consumers.

Here is a simplified architectural example of a compute IaaS platform:

![Simplified IaaS Architecture](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.1.3.1-1.png?raw=true)

A series of physical servers each run two components — a hypervisor (for virtualization), and the management/orchestration software to tie in the servers and connect them to the compute controller. A customer asks for an instance (virtual server) of a particular size and the cloud controller determines which server has the capacity, and allocates an instance of the requested size. 

The controller then creates a virtual hard drive by requesting storage from the storage controller, who allocates storage from the storage pool, and connects it to the appropriate host server and instance over the network (a dedicated network for storage traffic). Networking is also allocated, including virtual network interfaces, addresses, and connecting them to the necessary virtual network.

The controller then sends copy of the server image into the virtual machine, boots it, and configures it, creating an instance running in a virtual machine, with virtual networking and storage all properly configured. Once this entire process is complete, the metadata and connectivity information is brokered by the cloud controller and available to the consumer, who can now connect to the instance and log in.

#### 1.1.3.2 Platform as a Service

Of all the service models, **PaaS** is the hardest to definitively characterize due to both the wide range of PaaS offerings, and the many ways of building PaaS services. PaaS adds an additional layer of integration with application development frameworks, middleware capabilities, and functions such as database, messaging, and queuing. These services allow developers to build applications on the platform with programming languages and tools that are supported by the stack.

One option, frequently seen in the real world and illustrated in our model, is to build a platform on top of IaaS. A layer of integration and middleware is built on IaaS, then pooled together, orchestrated, and exposed to customers using APIs as PaaS. For example, a Database as a Service could be built by deploying modified database management system software on instances running in IaaS. The customer manages the database via API (and web console) and accesses it either through the normal database network protocols, or, again, via API. 

In PaaS the cloud consumer only sees the platform, not the underlying infrastructure. In our example, the database expands (or contracts) as needed based on utilization, without the customer having to manage individual servers, networking, patches, etc. 

Another example is an application deployment platform; a place where developers can load and run application code, without managing the underlying resources. Services exist for running nearly any kind of application in any language on PaaS, freeing the developers from configuring and building servers, keeping them up to date, or worrying about complexities like clustering and load balancing.

This simplified architecture diagram shows an application platform (PaaS) running on top of our IaaS architecture:

![Simplified PaaS Architecture](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.1.3.2-1.png?raw=true)

PaaS doesn't necessarily need to be built on top of IaaS; there is no reason it cannot be a custom-designed stand-alone architecture. The defining characteristic is that consumers access and manage the platform, not the underlying infrastructure (including cloud infrastructure).

#### 1.1.3.3 Software as a Service

SaaS services are full, multi-tenant applications, with all the architectural complexities of any large software platform. Many SaaS providers build on top of IaaS and PaaS due to the increased agility, resiliency and (potential) economic benefits. 

Most modern cloud applications (SaaS or otherwise) use a combination of IaaS and PaaS; sometimes across different cloud providers. Many also tend to offer public APIs for some (or all) functionality. They often need these to support a variety of clients, especially web browsers and mobile applications. 

Thus all SaaS tends to have an application/logic layer and data storage, with an API on top, then one or more presentation layers, often including web browsers, mobile applications, and public API access.

The simplified architecture diagram below is taken from a real SaaS platform, but generalized to remove references to the specific products in use:

<!-- SaaS architecture diagram -->

These reference and architectural models should not be construed as being canonical. They are included to provide security professionals a deeper understanding of how cloud computing works and is constructed, but there are far too many different approaches in the real world to include them all.

### 1.1.4 Logical Model

At a high level, both cloud and traditional computing adhere to a logical model that helps identify different layers based on functionality. This is useful to illustrate the differences between the different computing models themselves:

* *Infrastructure:* The core components of a computing system; compute, network, and storage. The foundation that every else is built on. The moving parts.
* *Metastructure:*  The protocols and mechanisms that provide the interface between the infrastructure layer and the other layers. The glue that ties the technologies and enables management and configuration.
* *Infostructure:* The data and information. Content in a database, file storage, etc.
* *Applistructure:* The applications and services used to build them.

![Cloud Logical Model](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.1.4-1.png?raw=true)

Different security focuses map to the different logical layers. Application security maps to applistructure, data security to infostructure, and infrastructure security to infrastructure. 

*The key difference between cloud and traditional computing is the metastructure*. In cloud, metastructure includes the management plane components, which in cloud, are network enabled and remotely accessible. Another key difference is that, in cloud, you tend to double up on each layer. Infrastructure, for example, includes both the infrastructure used to create the cloud, then the virtual infrastructure used and managed by the cloud consumer. In private cloud, the same organization might need to manage both, while in public cloud the provider manages the physical infrastructure, and the consumer their portion of the virtual infrastructure.

As we will discuss later, this has profound implications on who is responsible for, and manages, security. 

These layers tend to map to different teams, disciplines, and technologies commonly found in IT organizations. While the most obvious and immediate security management differences are in metastructure, cloud differs extensively from traditional computing within each layer. The scale of the differences will depend not only on the cloud platform, but on *how* exactly the cloud consumer utilizes the platform.

For example, a cloud-native application that makes heavy utilization of a cloud provider's PaaS products will experience more applistructure differences than the migration of an existing application, with minimal changes, to Infrastructure as a Service.

## 1.2 Cloud Security Scope, Responsibilities, and Models

### 1.2.1 Cloud Security and Compliance Scope and Responsibilities

It might sound trite, but cloud security and compliance includes everything you do today, just in the cloud. All the traditional security domains remain, but the *nature of risks*, *roles and responsibilities*, and *implementation of controls* change, often dramatically. 

While the overall scope of security and compliance doesn't change, the pieces any given cloud actor is responsible for most certainly does. Think of it this way — cloud computing is a shared technology model, where different organizations are frequently responsible for implementing and managing different parts of the stack. Thus security responsibilities are also distributed across the stack, and thus across the organizations involved.

This is commonly referred to as the *shared responsibility model*. Think of it as a responsibility matrix that depends on the particular cloud provider and feature/product, service model, and deployment model. 

At a high level, security responsibility maps to the degree of control any given actor has over the architecture stack:

* *Software as a Service:* The cloud provider is responsible for nearly all security, since the cloud consumer can only access and manage their use of the application, and can't alter how the application works. For example, a SaaS provider is responsible for perimeter security, logging/monitoring/auditing, and application security, while the consumer may only be able to manage authorization and entitlements.
* *Platform as a Service:* The cloud provider is responsible for the security of the platform, while the consumer is responsible for everything they implement on the platform, including how they configure any offered security features. The responsibilities are thus more evenly split. For example, when using a Database as a Service the provider manages fundamental security, patching, and core configuration, while the cloud consumer is responsible for everything else, including which security features of the database to use, managing accounts, or even authentication methods.
* *Infrastructure as a Service* Just like PaaS, the provider is responsible for foundational security, while the cloud consumer is responsible for everything they build on the infrastructure. Unlike PaaS, the line is lower, with far more responsibility on the client. For example, the IaaS provider will likely monitor their perimeter for attacks, but the consumer is fully responsible for how they define and implement their virtual network security based on the tools available on the service.

![Cloud Security Responsibilities](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.2.1-1.png?raw=true)

These roles are further complicated when using cloud brokers or other intermediaries and partners. 

*The most important security consideration is knowing exactly who is responsible for what for any given cloud project.* It's less important if any particular cloud provider offers a specific security control, as long as you know precisely what they do and how it works, and then you can fill the gaps with your own controls, or choose a different provider if you can't close the controls gap. Your ability to do this is very high for IaaS, and less so for SaaS.

This is the essence of the security relationship between a cloud provider and consumer. What does the provider do? What does the consumer need to do? Does the cloud provider enable the consumer to do what they need to? What is guaranteed in the contract and service level agreements, and what is implied by the documentation and specifics of the technology?

The shared responsibility directly correlates to two recommendations:

* *Cloud providers* should clearly document their internal security controls and customer security features so the cloud consumer can make an informed decision, and properly design and implement those controls.
* *Cloud consumers* should, for any given cloud project, build a responsibilities matrix to document who is implementing which controls and how. This should also align with any necessary compliance standards.

The Cloud Security Alliance provides two tools to help meet these requirements:

* The [Consensus Assessment Initiative Questionnaire (CAIQ)][8]. A standard template for cloud providers to document their security and compliance controls.
* The [Cloud Controls Matrix (CCM)][9]which lists cloud security controls, and maps them to multiple security and compliance standards. The CCM can also be used to document security responsibilities. 

[8]: https://cloudsecurityalliance.org/group/consensus-assessments/
[9]: https://cloudsecurityalliance.org/group/cloud-controls-matrix/

Both documents will need tuning for specific organizational and project requirements, but provide a comprehensive starting template, and can be especially useful for ensuring compliance requirements are met.

### 1.2.2 Cloud Security Models

Cloud security models are tools to help guide security decisions. The term "model" can be used a little nebulously so for our purposes we break out the following types:

* *Conceptual models or frameworks* include visualizations and descriptions used to explain cloud security concepts and principles, such as the CSA logical model in this document.
* *Controls models or frameworks* categorize and detail specific cloud security controls or categories of controls, such as the CSA CCM. 
* *Reference architectures* are templates for implementing cloud security, typically generalized (e.g. an IaaS security reference architecture). They can be very abstract, bordering on conceptual, or quite detailed, down to specific controls and functions.
* *Design patterns* are reusable solutions to particular problems. In security, an example is IaaS log management. As with reference architectures, they can be more or less abstract or specific, even down to common implementation patterns on particular cloud platforms.

The lines between these models often blur and overlap, depending on the goals of developer of the model. Even lumping these all together under the heading "model" is probably inaccurate, but since we see the terms used so interchangeably across different sources, it makes sense to group them.

The CSA has reviewed and recommends the following models:

* The [CSA Enterprise Architecture][10]
* The CSA [Cloud Controls Matrix (CCM)][11]
* The NIST draft [Cloud Computing Security Reference Architecture (NIST Special Publication 500-299)][12] , which includes conceptual models, reference architectures, and a controls framework.
* [ISO/IEC FDIS 27017 Informatiwon technology -- Security techniques -- Code of practice for information security controls based on ISO/IEC 27002 for cloud services][13].

[10]: https://research.cloudsecurityalliance.org/tci/index.php/explore/
[11]: https://cloudsecurityalliance.org/group/cloud-controls-matrix/
[12]: http://collaborate.nist.gov/twiki-cloud-computing/pub/CloudComputing/CloudSecurity/NIST_Security_Reference_Architecture_2013.05.15_v1.0.pdf
[13]: http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?ics1=35&ics2=040&ics3=&csnumber=43757

![CSA Enterprise Architecture](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.2.2-1.png?raw=true)

Throughout this Guidance we also refer to other domain-specific models.

#### 1.2.2.1 A Simple Cloud Security Process Model

While the implementation details, necessary controls, specific processes, and various reference architectures and design models vary greatly depending on the specific cloud project, there is a relatively straightforward, high-level process for managing cloud security:

1. Identify necessary security and compliance requirements, and any existing controls.
2. Select your cloud provider, service, and deployment models.
3. Define the architecture.
4. Assess the security controls.
5. Identify control gaps.
6. Design and implement controls to fill the gaps.
7. Manage changes over time.

Since different cloud projects, even on a single provider, will likely leverage entirely different sets of configurations and technologies, each project should be evaluated on its own merits. For example, the security controls for an application deployed on pure IaaS in one provider may look very different than a similar project that instead uses more PaaS from that same provider.

The key is to identify requirements, design the architecture, and then identify the gaps based on the capabilities of the underlying cloud platform. That's why you need to know the cloud provider and architecture *before* you start translating security requirements into controls.

![Cloud Security Process](https://github.com/cloudsecurityalliance/CSA-Guidance/blob/master/Images/1.2.2.1-1.png?raw=true)

## 1.3 Areas of Critical Focus

The thirteen other domains which comprise the remainder of the CSA guidance highlight areas of concern for cloud computing and are tuned to address both the strategic and tactical security ‘pain points’ within a cloud environment and can be applied to any combination of cloud service and deployment model. 

The domains are divided into two broad categories: governance and operations.  The governance domains are broad and address strategic and policy issues within a cloud computing environment, while the operational domains focus on more tactical security concerns and implementation within the architecture.

### 1.3.1 Governing in the Cloud

|	Domain	|	Title		|	Description	|
|	----------	|	-----------	|	--------------	|
|	2	|	Governance and Enterprise Risk Management		|	The ability of an organization to govern and measure enterprise risk introduced by cloud computing.  Items such as legal precedence for agreement breaches, ability of user organizations to adequately assess risk of a cloud provider, responsibility to protect sensitive data when both user and provider may be at fault, and how international boundaries may affect these issues.	|
|	3	|	Legal Issues: Contracts and Electronic Discovery	|	Potential legal issues when using cloud computing.  Issues touched on in this section include protection requirements for information and computer systems, security breach disclosure laws, regulatory requirements, privacy requirements, international laws, etc.	|
|	4	|	Compliance and Audit Management	|	Maintaining and proving compliance when using cloud computing. Issues dealing with evaluating how cloud computing affects compliance with internal security policies, as well as various compliance requirements (regulatory, legislative, and otherwise) are discussed here.  This domain includes some direction on proving compliance during an audit.	|
|	5	|	Information Governance 	|	Governing data that is placed in the cloud.  Items surrounding the identification and control of data in the cloud, as well as compensating controls that can be used to deal with the loss of physical control when moving data to the cloud, are discussed here. Other items, such as who is responsible for data confidentiality, integrity, and availability are mentioned. 	|

### 1.3.2 Operating in the Cloud

|	Domain	|	Title		|	Description	|
|	----------	|	-----------	|	--------------	|
|	6	|	Management Plane and Business Continuity 	|	Securing the management plane and administrative interfaces used when accessing the cloud, including both web consoles and APIs. Ensuring business continuity for cloud deployments.	|
|	7	|	Infrastructure Security 	|	Core cloud infrastructure security, including networking, workload security, and hybrid cloud considerations. This domain also includes security fundamentals for private clouds.	|
|	8	|	Virtualization and Containers	|	Security for hypervisors, containers, and Software Defined Networks.	|
|	9	|	Incident Response, Notification and Remediation	|	Proper and adequate incident detection, response, notification, and remediation.  This attempts to address items that should be in place at both provider and user levels to enable proper incident handling and forensics.  This domain will help you understand the complexities the cloud brings to your current incident-handling program. 	|
|	10	|	Application Security	|	Securing application software that is running on or being developed in the cloud.  This includes items such as whether it’s appropriate to migrate or design an application to run in the cloud, and if so, what type of cloud platform is most appropriate (SaaS, PaaS, or IaaS). 	|
|	11	|	Data Security and Encryption	|	Implementing data security and encryption, and ensuring scalable key management. 	|
|	12	|	Identity, Entitlement, and Access Management	|	Managing identities and leveraging directory services to provide access control.  The focus is on issues encountered when extending an organization’s identity into the cloud.  This section provides insight into assessing an organization’s readiness to conduct cloud-based Identity, Entitlement, and Access Management (IdEA). 	|
|	13	|	Security as a Service	|	Providing third party facilitated security assurance, incident management, compliance attestation, and identity and access oversight. 	|
|	14	|	Related Technologies	|	Established and emerging technologies with a close relationship to cloud computing, including Big Data, Internet of Things, and mobile computing.	|

## 1.4 Recommendations

1. Understand the differences between cloud computing and traditional infrastructure or virtualization, and how *abstraction* and *automation* impact security.
1. Become familiar with the NIST model for cloud computing, and the CSA reference architecture.
1. Use tools like the CSA Common Assessment Initiative Questionnaire (CAIQ) to evaluate and compare cloud providers.
1. Cloud providers should clearly document their security controls and features and publish them using tools like the CSA CAIQ
1. Use tools like the CSA Cloud Controls Matrix (CCM) to assess and document cloud project security and compliance requirements and controls, and who is responsible for each.
1. Use a cloud security process model to select providers, design architectures, identify control gaps, and implement security and compliance controls.

## 1.5 Credits

* Rich Mogull
* Reference architecture and logical model based on the work of Christofer Hoff
