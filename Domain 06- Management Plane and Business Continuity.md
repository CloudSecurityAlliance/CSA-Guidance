
# Domain 6: Management Plane and Business Continuity

## Introduction

The management plane is the single most significant security difference between traditional infrastructure and cloud computing. This isn't all of the metastructure (defined in Domain 1) but is the interface to connect with the metastructure and configure much of the cloud.

We always have a management plane, the tools and interfaces we use to manage our infrastructure, platforms, and applications, but cloud abstracts and centralizes administrative management of resources. Instead of controlling a data center configuration with boxes and wires, it is now controlled with API calls and web consoles.

Thus gaining access to the management plane is like gaining unfettered access to your data center, unless you put the proper security controls in place to limit who can access the management plane and what they can do within it. 

To think about it in security terms, the management plane consolidates many things we previously managed through separate systems and tools, and then makes them Internet-accessible with a single set of authentication credentials. This isn't a net loss for security—there are also gains—but it is most definitely different, and it impacts how we need to evaluate and manage security. 

Centralization also brings security beneifts. There are no hidden resources: you always know where everything *you own* is at all times, and how it is configured. This is an emergent property of both broad network access and metered service. The cloud controller always needs to know what resources are in the pool, out of the pool, and where they are allocated.

This doesn't mean that all the assets you put into the cloud are equally managed. The cloud controller can't peer into running servers or open up locked files, nor understand the implications of your specific data and information.

In the end, this is an extension of the shared responsibility model discussed in Domain 1 and throughout this Guidance. The cloud management plane is responsible for managing the assets of the resource pool, while the cloud consumer is responsible for how they configure those assets, and for the assets they deploy into the cloud.

* The cloud provider is responsible for ensuring the management plane is secure and *necessary security features are exposed to the cloud consumer*, such as granular entitlements to control what someone can do even if they have management plane access.

* The cloud consumer is responsible for properly configuring their use of the management plane, as well as for securing and managing their credentials.

### Business continuity and disaster recovery in the cloud
	
BC/DR is just as important in cloud computing as it is for any other technology. Aside from the differences resulting from the potential involvement of a third-party provider (something we often deal with in BC/DR), there are additional considerations due to the inherent differences when using shared resources.

The three main aspects of BC/DR in the cloud are: 

* Ensuring continuity and recovery within a given cloud provider. These are the tools and techniques to best architect your cloud deployment to keep things running if either what you deploy breaks, or a portion of the cloud provider breaks.

* Preparing for and managing cloud provider outages. This extends from the more constrained problems that you can architect around within a provider to the wider outages that take down all or some of the provider in a way that exceeds the capabilities of inherent DR controls.

* Considering options for portability, in case you need to migrate providers or platforms. This could be due to anything from desiring a different feature set to the complete loss of the provider if, for example, they go out of business or you have a legal dispute.

#### Architect for failure

Cloud platforms can be incredibly resilient, but single cloud assets are typically less resilient than in the case of traditional infrastructure. This is due to the inherently greater fragility of virtualized resources running in highly-complex environments.

This mostly applies to compute, networking, and storage, since those allow closer to raw access, and cloud providers can leverage additional resiliency techniques for their platforms and applications that run on top of IaaS.

However, this means that cloud providers tend to offer options to improve resiliency, often beyond that which is attainable (for equivalent costs) in traditional infrastructure. For example, by enabling multiple "zones" where you can deploy virtual machines within an auto-scaled group that encompasses physically distinct data centers for high-availability. Your application can be balanced across zones so that if an entire zone goes down your application still stays up. This is quite difficult to implement in a traditional data center, where it typically isn't cost-effective to build multiple, isolated physical zones across which you can deploy a cross-zone load-balanced application with automatic failover.

But this extra resiliency is only true if you architect to leverage these capabilities. Deploying your application all in one zone, or even on a single virtual machine in a single zone, is likely to be less resilient than deploying on a single, well-maintained physical server.

This is why "lift and shift" wholesale migration of existing applications without architectural changes can *reduce* resiliency. Existing applications are rarely architected and deployed to work with these resiliency options, yet straight-up virtualization and migration without changes can increase the odds of individual failures. 

The ability to manage is higher with IaaS and much lower with SaaS, just like security. For SaaS you rely on the cloud provider keeping the entire application service up. With IaaS you can architect *your* application to account for failures, putting more responsibility in your hands. PaaS, as usual, is in the middle—some PaaS may have resiliency options that you can configure, while other platforms are completely in the hands of the provider.

Overall, a risk-based approach is key:
* Not all assets need equal continuity.
* Don't drive yourself crazy by planning for full provider outages just because of the perceived loss of control. Look at historical performance,
* Strive to design for RTOs and RPOs equivalent to those on traditional infrastructure,

## Overview

### Management Plane Security

The management plane refers to the interfaces for managing your assets in the cloud. If you deploy virtual machines on a virtual network the management plane is how you launch those machines and configure that network. For Software as a Service the management plane is often the "admin" tab of the user interface and where you configure things like users, settings for the organization, etc.

The management plane controls and configures the metastructure (defined in Domain 1), and is also part of the metastructure itself. As a reminder, cloud computing is taking physical assets (like networks and processors) and using them to build resource pools. Metastructure is the glue and guts to create, provision, and deprovision the pools. The management plane includes the interfaces for building and managing the cloud itself, but also the interfaces for cloud consumers to manage their own allocated resources of the cloud.

The management plane is a key tool for enabling and enforcing separation and isolation in multitenancy. Limiting who can do what with the APIs is one important means for segregating out customers, or different users within a single tenant.

#### Accessing the management plane

APIs and web consoles are the way the management plane is delivered. Application Programming Interfaces allow for programatic management of the cloud. They are the glue that holds the cloud's components together and enables their orchestration. Since not everyone wants to write programs to manage their cloud, web consoles provide visual interfaces. In many cases web consoles merely use the same APIs you can access directly.

Cloud providers and platforms will also often offer Software Development Kits (SDKs) and Command Line Interfaces (CLIs) to make integrating with their APIs easier.

* *Web consoles* are managed by the provider. They can be organization-specific (typically using DNS redirection tied to federated identity). For example, when you connect to your cloud file-sharing application you are redirected to your own "version" of the application after you log in. This version will have its own domain name associated with it, which allows you to integrate more easily with federated identity (e.g. instead of all your users logging into "application.com" they log into "your-organization.application.com").

As mentioned, most web consoles offer a user interface for the same APIs that you can access directly. Although, depending on the platform or provider's development process, you may sometimes encounter a mismatch where either a web feature or an API call appear on one before the other.

* *APIs* are typically [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) for cloud services, since REST is easy to implement across the Internet. REST APIs have become the standard for web-based services since they run over HTTP/S and thus work well across diverse environments.

These can use a variety of authentication mechanisms, as there is no single standard for authentication in REST. HTTP request signing and OAuth are the most common; both of these leverage cryptographic techniques to validate authentication requests.

You still often see services that embed a password in the request. This is less secure and at higher risk for credential exposure. It's most often seen in older or poorly-designed web platforms that built their web interface first and only added consumer APIs later. If you do encounter this, you need to use dedicated accounts for API access if possible, in order to reduce the opportunities for credential exposure.

#### Securing the management plane

Identity and Access management (IAM) includes identification, authentication, and authorizations (including access management). This is how you determine who can do what within your cloud platform or provider.

The specific options, configurations, and even concepts vary heavily between cloud providers and platforms. Each has their own implementation and may not even use the same definitions for things like "groups" and "roles." 

No matter the platform or provider there is always an account owner with super-admin privileges to manage the entire configuration. This should be enterprise-owned (not personal), tightly locked down, and nearly never used.

Separate from the account-owner you can usually create super-admin accounts for individual admin use. Use these privileges sparingly; this should also be a smaller group since compromise or abuse of one of these accounts could allow someone to change or access essentially everything and anything. 

Your platform or provider may support lower-level administrative accounts that can only manage parts of the service. We sometimes call these "service administrators" or "day to day administrators". These accounts don't necessarily expose the entire deployment if they are abused or compromised and thus are better for common daily usage. They also help compartmentalize individual sessions, so it isn't unusual to allow a single human administrator access to multiple service administrator accounts (or roles) so they can log in with just the privileges they need for that particular action instead of having to expose a much wider range of entitlements.

Both providers and consumers should consistently only allow the least privilege required for users, applications, and other management plane usage. 

All privileged user accounts should use multifactor authentication (MFA). If possible, *all* cloud accounts (even individual user accounts) should use MFA. It's one of the single most effective security controls to defend against a wide range of attacks. This is also true regardless of the service model: MFA is just as important for SaaS as it is for IaaS.

(See the IAM domain for more information on IAM and the role of federation and strong authentication, much of which applies to the cloud management plane.)

#### Management plane security when building/providing a cloud service

When you are responsible for building and maintaining the management plane itself, such as in a private cloud deployment, that increases your responsibilities. When you consume the cloud you only configure the parts of the management plane that the provider exposes to you, but when you are the cloud provider you obviously are responsible for everything.

Delving into implementation specifics is beyond the scope of this Guidance, but at a high level there are five major facets to building and managing a secure management plane:

* *Perimeter security:* Protecting from attacks against the management plane's components itself, such as the web and API servers. It includes both lower-level network defenses as well as higher-level defenses against application attacks.

* *Customer authentication:* Providing secure mechanisms for customers to authenticate to the management plane. This should use existing standards (like OAuth or HTTP request signing) that are cryptographically valid and well documented. Customer authentication should support MFA as an option or requirement.

* *Internal authentication and credential passing:* The mechanisms your own employees use to connect with the non-customer-facing portions of the management plane. It also includes any translation between the customer's authentication and any internal API requests. Cloud providers should always mandate MFA for cloud management authentication.

* *Authorization and entitlements:* The entitlements available to customers and the entitlements for internal administrators. Granular entitlements better enable customers to securely manage their own users and administrators. Internally, granular entitlements reduce the impact of administrators' accounts being compromised or employee abuse.

* *Logging, monitoring, and alerting:* Robust logging and monitoring of administrative is essential for effective security and compliance. This applies both to what the customer does in their account, and to what employees do in their day-to-day management of the service. Alerting of unusual events is an important security control to ensure that monitoring is actionable, and not merely something you look at after the fact. Cloud customers should ideally be able to access logs of their own activity in the platform via API or other mechanism in order to integrate with their own security logging systems.

### Business continuity and disaster recovery

Like security and compliance, business continuity and disaster recovery (BC/DR) is a shared responsibility. There are aspects that the cloud provider has to manage, but the cloud customer is also ultimately responsible for how they use and manage the cloud service. This is especially true when planning for outages of the cloud provider (or parts of the cloud provider's service). 

Also similar to security, customers have more control and responsibility in IaaS, less in SaaS, with PaaS in the middle.

BC/DR must take a risk-based approach. Many BC options may be cost prohibitive in the cloud, but may also not be necessary. This is no different than in traditional data centers, but it isn't unusual to want to over-compensate when losing physical control. For example, the odds of a major IaaS provider going out of business or changing their entire business model is low, but this isn't all that uncommon for a smaller venture-backed SaaS provider.

* Ask the provider for outage statistics over time since this can help inform your risk decisions. 

* Remember that capabilities vary between providers and should be included in the vendor selection process. 
        
#### Business continuity within the cloud provider

When you deploy assets into the cloud you can't assume the cloud will always be there, or always work the way you expect. Outages and issues are no more or less common than with any other technology, although the cloud can be overall more resilient when the provider includes mechanisms to better enable building resilient applications. 

This is a key point we need to spend a little more time on: As we've mentioned in a few places the very nature of virtualizing resources into pools typically creates *less* resiliency for any single asset, like a virtual machine. On the other hand, abstracting resources and managing everything through software opens up flexibility to more-easily enable resiliency features like durable storage and cross-geographic load balancing. 

There is a huge range of options here, and not all providers or platforms are created equal, but you shouldn't assume that "the cloud" as a general term is more or less resilient than traditional infrastructure. Sometimes it's better, sometimes it's worse, and knowing the difference all comes down to your risk assessment and *how* you use the cloud service. 

This is why it is typically best to re-architect deployments when you migrate them to the cloud. Resiliency itself, and the fundamental mechanisms for ensuring resiliency, change. Direct "lift and shift" migrations are less likely to account for failures, nor will they take advantage of potential improvements from leveraging platform or service specific capabilities.

The focus is on understanding and leveraging the platform's BC/DR features. Once you make the decision to deploy in the cloud you then want to optimize your use of included BC/DR features before adding on any additional capabilities through third-party tools.

BC/DR must account for the entire logical stack:

* Metastructure

Since cloud configurations are controlled by software, these configurations should be backed up in a restorable format. This isn't always possible, and is pretty rare in SaaS, but there are tools to implement this in many IaaS platforms (including third-party options) using *Software Defined Infrastructure*. 

*Software Defined Infrastructure* allows you to create an infrastructure template to configure all or some aspects of a cloud deployment. These templates are then translated natively by the cloud platform or into API calls that orchestrates the configuration. 

This should include controls like IAM and logging, not merely architecture, network design, or service configurations.

* Infrastructure

As mentioned, any provider will offer features to support higher availability than can comparably be achieved in a traditional data center for the same cost. But these only work if you adjust your architecture. "Lifting and shifting" applications to the cloud without architectural adjustments or redesign will often result in lower availability.

Be sure and understand the cost model for these features—especially for implementing them across the provider's physical locations/regions, where the cost can be high. Some assets and data must be converted to work across cloud locations/regions, for example, custom machine images used to launch servers. These assets must be included in plans.

* Infostructure

Data synchronization is often one of the more difficult issues to manage across locations, even if the actual storage costs are manageable. This is due to the size of data sets (vs. an infrastructure configuration) and keeping data in sync across locations and services, something that's often difficult even in a single storage location/system. 

* Applistructure

Applistructure includes all of the above, but also the application assets like code, message queues, etc. When a cloud consumer builds their own cloud applications it's usually built on top of IaaS and/or PaaS, so resiliency and recovery are inherently tied to those layers. But Applistructure includes the full range of everything in an application.

Understand PaaS limitations and lock-ins, and plan for the outage of a PaaS component. Platform services include a range of functions we used to manually implement in applications, everything from authentication systems to message queues and notifications. It isn't unusual for modern applications to even integrate these kinds of services from multiple different cloud providers, creating an intricate web.

Discussing availability of the component/service with your providers is reasonable. For example, the database service from your infrastructure provider may not share the same performance and availability as their virtual machine hosting.

When real-time switching isn't possible, design your application to gracefully fail in case of a service outage. There are many automation techniques to support this. For example, if your queue service goes down, that should trigger halting the front end so messages aren't lost.

Downtime is always an option. You don't always need perfect availability, but if you *do* plan to accept an outage you should at least ensure you fail gracefully, with emergency downtime notification pages and responses. This may be possible using static stand-by via DNS redirection.

"Chaos Engineering" is often used to help build resilient cloud deployments. Since everything cloud is API-based, Chaos Engineering uses tools to selectively degrade portions of the cloud to continuously test business continuity. 

This is often done in production, not just test environment, and forces engineers to assume failure instead of viewing it as only a possible event. By *designing systems for failure you can better absorb individual component failures*.

#### Business continuity for loss of the cloud provider

It is always possible that an entire cloud provider, or at least a major portion of their infrastructure (such as one specific geography) can go down. Planning for cloud provider outages is difficult, due to the natural lock-in of leveraging a provider's capabilities. Sometimes you can migrate to a different portion of their service, but in other cases an internal migration simply isn't an option, or you may be totally locked in.

Depending on the history of your provider, and their internal availability capabilities, accepting this risk is often a legitimate option. 

Downtime may be another option, but it depends on your recovery time objectives (RTO). However, some sort of static stand-by should be available via DNS redirection. Graceful failure should also include failure responses to API calls, if you offer APIs.

Be wary of selecting a secondary provider or service if said service may also be located or reliant on the same provider. It doesn't do you any good to use a backup storage provider if said provider happens to be based on the same infrastructure provider.

Moving data between providers can be difficult, but might be easy compared to moving metastructure, security controls, logging, and so on, which may be incompatible between platforms.

SaaS may often be the biggest provider outage concern, due to total reliance on the provider. Scheduled data extraction and archiving may be your only BC option outside of accepting downtime. Extracting and archiving to another cloud service, especially IaaS/PaaS, may be a better option than moving it to local/on-premise storage. Again, take a risk-based approach that includes the unique history of your provider.

Even if you have your data, you must have an alternate application that you know you can migrate it into. If you can't use the data, you don't have a viable recovery strategy.

Test, test, and test. This may often be easier than in a traditional data center because you aren't constrained by physical resources, and only pay for use of certain assets during the life of the test.

#### Business continuity for private cloud and providers

This is completely on on the provider's shoulders, and BC/DR includes everything down to the physical facilities. RTOs and RPOs will be stringent, since if the cloud goes down, everything goes down. 

If you are providing services to others, be aware of contractual requirements, including data residency, when building your BC plans. For example, failing over to a different geography in a different legal jurisdiction may violate contracts or local laws.

## Recommendations

* Management plane (metastructure) security
    * Ensure there is strong perimeter security for API gateways and web consoles.
    * Use strong authentication and MFA.
    * Maintain tight control of primary account holder/root account credentials and consider dual-authority to access them.
        * Establishing multiple accounts with your provider will help with account granularity and to limit blast radius (with IaaS and PaaS).
    * Use separate super administrator and day-to-day administrator accounts instead of root/primary account holder credentials.
    * Consistently implement least privilege accounts for metastructure access.
        * This is why you separate development and test accounts with your cloud provider.
    * Enforce use of MFA whenever available.
* Business continuity
    * Architecture for failure.
    * Take a risk-based approach to everything. Even when you assume the worst, it doesn't mean you can afford or need to keep full availability if the worst happens.
    * Design for high availability within your cloud provider. In IaaS and PaaS this is often easier and more cost effective than the equivalent in traditional infrastructure.
        * Take advantage of provider-specific features.
        * Understand provider history, capabilities, and limitations.
        * Cross-location should always be considered, but beware of costs depending on availability requirements.
            * Also ensure things like images and asset IDs are converted to work in the different locations.
        * BC for metastructure is as important as that for assets.
    * Prepare for graceful failure in case of a cloud provider outage.
		* This can include plans for interoperability and portability with other cloud providers or a different region with your current provider.
    * For super-high-availability applications, start with cross-location BC before attempting cross-provider BC.
    * Cloud providers, including private cloud, must provide the highest levels of availability and mechanisms for customers/users to manage aspects of their own availability.