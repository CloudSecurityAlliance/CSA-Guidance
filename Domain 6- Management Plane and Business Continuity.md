
# Domain 6: Management Plane and Business Continuity


## Introduction

* The importance of the management plane (metastructure).
    * The management plane is the single most significant security difference between traditional infrastructure and cloud computing.
        * We always have a management plane, but cloud abstracts and centralizes administrative management of resources. Instead of controlling a data center configuration with boxes and wires, it is now controlled with API calls and web consoles.
        * Thus gaining access to the management plane is like gaining unfettered access to your data center, unless you put the proper security controls in place.
    * It also brings security benefits through this centralization. There are no hidden resources, you always know where everything *you own* is at all times, and how it is configured.
        * This is an emergent property of both broad network access and metered service. The cloud controller always needs to know what resources are in the pool, out of the pool, and where they are allocated.
        * This doesn't mean all the assets you put into the cloud are equally managed. The cloud controller can't peer into running servers or open up locked files, nor understand the implications of your specific data and information.
    * This is an extension of the shared responsibility model. The cloud management plane is responsible for managing the assets of the resource pool, while the cloud consumer is responsible for how they configure those assets, and for the assets they deploy into the cloud.
    * The cloud provider is responsible for ensuring the management plane is secure and *necessary security features are exposed to the cloud consumer*.
    * The cloud consumer is responsible for properly configuring their use of the management plane and for securing and managing their access credentials.
* Business continuity in the cloud
    * Three aspects
        * Ensuring continuity within a given cloud provider.
        * Preparing for and managing cloud provider outages.
        * Considering options for portability.
    * Architect for failure
        * Cloud platforms can be incredibly resilient, but single cloud assets are typically less resilient than with traditional infrastructure.
            * Mostly applies to compute, networking, and storage,
        * However, this means cloud providers tend to offer options to improve resiliency, often beyond that attainable (for equivalent costs) in traditional infrastructure.
            * This is only true if you architect to leverage these capabilities.
            * This is why "lift and shift" wholesale migration of existing applications without architectural changes can *reduce* resiliency.
        * Ability to manage is higher with IaaS and much lower with SaaS. Just like security.
    * A risk-based approach is key.
        * Not all assets need equal continuity.
        * Don't drive yourself crazy by planning for full provider outages just because of the perceived loss of control. Look at historical performance,
        * Strive to design for RTOs and RPOs equivalent to those on traditional infrastructure,

## Overview

* Management Plane Security
    * Defining the management plane
        * The interfaces for managing your assets in the cloud.
        * Controls and configures the metastructure
        * Part of metastructure.
        * Delivered via APIs and web consoles.
    * Accessing the management plane
        * Web consoles
            * Managed by the provider.
            * Can be organization-specific (typically using DNS redirection tied to federated identity).
        * APIs
            * Typically REST for cloud services
            * Can use a variety of authentication mechanisms
                * HTTP request signing and OAuth most common
                * Still often see services that embed password in request.
                    * If so, need to use dedicated API accounts if possible.
    * Securing the management plane
        * IAM
            * Varies between cloud providers
            * There is always an account owner with super-admin. This should be enterprise-owned (not personal), tightly locked down, and nearly never used.
            * Create super admin accounts for individual admin use, but this should also be a smaller group.
            * Then, if possible, use "day to day" admin accounts with tighter restrictions.
            * All privileged user accounts should use MFA
                * If possible, all cloud accounts period should use MFA. It's one of the single most effective security controls to defend against a wide range of attacks.
            * See the IAM domain for more information on IAM and the role of federation and strong authentication, much of which applies to the cloud management plane.
        * Consistently maintain least privilege
        * Management plane security when building/providing a cloud service (you are responsible for delivering the management plane).
            * Five major facets.
                * Perimeter security
                * Customer authentication
                * Internal authentication and credential passing
                * Authorization and entitlements
                * Logging, monitoring, and alerting
* Business continuity
    * Once again, a shared responsibility
        * Similar to security, customers have more control and responsibility in IaaS, less in SaaS, with PaaS in the middle.
        * Must take a risk-based approach. Many BC options may be cost prohibitive in the cloud, but may also not be necessary. This is no different than in traditional data centers, but it isn't unusual to want to over-compensate when losing physical control.
            * Ask the provider for outage statistics over time.
        * Capabilities vary between providers and should be included in the vendor selection process.
    * Business continuity within the cloud provider
        * Focus is on understanding and leveraging the platforms BC/DR features.
        * BC/DR must include the entire logical stack.
            * Metastructure.
                * Since cloud configurations are controlled by software, these configurations must be backed up in a restorable format.
                * This should include controls like IAM and logging, not merely architecture, network design, or service configurations.
            * Infrastructure.
                * Many providers offer features to support higher availability than can comparably be achieved in a traditional data center for the same cost. But these only work if you adjust your architecture. "Lifting and shifting" applications to the cloud without architectural adjustments or redesign will often result in lower availability.
                    * Understand the cost model for these features. Especially for implementing them across the provider's physical locations/regions, where the cost can be high.
                * Some assets and data must be converted to work across cloud locations/regions. For example, custom machine images used to launch servers. These assets much be included in plans.
            * Infostructure
                * Data synchronization is often one of the more difficult issues to manage across locations, even if the actual storage costs Are manageable.
            * Applistructure
                * Application structure includes all of the above, but also the app assets like code, message queues, etc.
                * Understand PaaS limitations and lock ins, and plan for outage of a PaaS component.
                    * Discussing availability of the component/service with your provider is reasonable.
                    * When real-time switching isn't possible, design your application to gracefully fail in case of a service outage. There are many automation techniques to support this. For example, if y9ur queue service goes down, that should trigger halting the front end so messages aren't lost.
                * Downtime is always an option.
        * Will require automation for consistency.
        * Cloud BC and DR are as much about high-availability architecture as backup and restore. Many of the mechanisms used for elasticity can be leveraged to improve resiliency and provide fast failover.
    * Business continuity for loss of the cloud provider
        * Planning for cloud provider outages is difficult due to the natural lock in of leveraging a provider's capabilities.
            * Depending on the history of your provider, and their internal availability capabilities, accepting this risk is often a legitimate option.
        * Downtime may be an option, it depends on your RTOs.
            * However, some sort of static stand-by should be available via DNS redirection.
            * This should include API calls for mobile, partner, and other apps and services, or at least graceful failure and possibly proactive notifications using a secondary provider.
        * Be wary of selecting a secondary provider or service if said service may also be located or reliant on the same provider.
        * Moving data is easy compared to moving metastructure, security controls, logging, etc.
        * SaaS may often be the biggest concern, due to total reliance on the provider. Scheduled data extraction and archiving may become critical.
            * Extracting and archiving to another cloud service, especially IaaS/PaaS, may be a better option than moving it locally.
            * Again, take a risk-based approach that includes the unique history of your provider,
    * Business continuity for private cloud and providers
        * Completely on your shoulders.
        * RTOs and RPOs will be stringent.
        * When providing services to others, be aware of contractual requirements, including data residency,
    * Test, test, and test. This may often be easier than in a traditional data center because you aren't constrained by physical resources, and only pay for use of certain assets during the life of the test.

## Recommendations

* Management plane (metastructure) security
    * Ensure there is strong perimeter security for API gateways and web consoles.
    * Use strong authentication and MFA .
    * Maintain tight control of primary account holder/root account credentials and consider dual-authority to access them.
        * Establishing multiple accounts with your provider will help with account granularity and to limit blast edits (with IaaS and PaaS).
    * Use separate super administrator and day to day administrator accounts instead of root/primary account holder credentials.
    * Consistently implement least privilege accounts for metastructure access.
        * This is why separate development and test accounts with your cloud provider
    * A force use of MFA whenever available.
* Business continuity
    * Architecture for failure.
    * Take a risk-based approach to everything. Even when you assume the worst, it doesn't mean you can afford or need to keep full availability if the worst happens.
    * Design for high availability within your cloud provider. In IaaS and PaaS this is often easier and more cost effective than the equivalent in traditional infrastructure.
        * Take advantage of provider-specific features.
        * Understand provider history, capabilities, and limitations.
        * Cross-location should always be considered, but beware of costs depending on availability requirements.
            * Also ensure things like images and asset IDs are converted to work in the different locations,
        * BC for metastructure is as important as that for assets.
    * Prepare for graceful failure in case of a cloud provider outage.
    * For super-high-availability applications start with cross-location BC before attempting cross-provider BC.
    * Cloud providers, including private cloud, must provide the highest levels of availability and mechanisms for customers/users to manage aspects of their own availability.
