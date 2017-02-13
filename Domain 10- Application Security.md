# Domain 10: Application Security

## Introduction

Application security encompasses an incredibly complex and large body of knowledge: everything from early design and threat-modeling down to maintaining and defending production applications. Application security is also evolving at an incredibly rapid pace as the fundamentals of application development continue to progress and embrace new processes, patterns, and technologies. Cloud computing is one of the biggest drivers of these advancements and that results in corresponding pressure to evolve the state of application security, in order to ensure that this progress continues as safely as possible.

This section of the guidance is intended for software development and IT teams who want to securely build and deploy applications in cloud computing environments, specifically PaaS and IaaS (and many of these techniques are used to underpin secure SaaS applications). It focuses on:

* How application security differs in cloud computing.
* Reviewing secure software development basics and how those change in the cloud.
* Leveraging cloud capabilities for more secure cloud applications.

We can't cover all possible development and deployment options—even just the ones directly related to cloud computing—so the goal is to focus on significant areas that should help guide security in the majority of situations. This domain also introduces security fundamentals for DevOps, which is rapidly emerging as a dominant force in cloud-based application development.
	
Cloud computing mostly brings security benefits to applications, but as with most areas of cloud technology, it does require commensurate changes to existing practices, processes, and technologies which were not designed to operate in the cloud. At a high level this balance of opportunities and challenges includes:

### Opportunities

* *Higher baseline security.* Cloud providers, especially major IaaS and PaaS providers, have significant economic incentives to maintain higher baseline security than most organizations. In a cloud environment, major baseline security failures completely undermine the trust that a public cloud provider needs in order to maintain relationships with its customer base. Cloud providers are also subject to a wider range of security requirements, in order to meet all the regulatory and industry compliance baselines needed to attract customers from those verticals. 

* *Responsiveness.* APIs and automation provide extensive flexibility to build more-responsive security programs at a lower cost than in traditional infrastructure. For example, changing firewall rules or deploying new servers with updated code can be handled with a few API calls or through automation.

* *Isolated environments.* Cloud applications can also leverage virtual networks and other structures, including PaaS, for hyper-segregated environments. For example, it is possible, at no additional cost, to deploy multiple application stacks on entirely separate virtual networks, eliminating the ability for an attacker to use one compromised application to attack others behind the perimeter firewalls.

* *Independent virtual machines.* Security is further enhanced by the use of micro-service architectures. Since cloud doesn't require the consumer to optimize the use of physical servers, a requirement that often results in deploying multiple application components and services on a single system, developers can instead deploy more, smaller virtual machines, each dedicated to a function or service. This reduces the attack surface of the individual virtual machines and supports more granular security controls.

* *Elasticity.* Elasticity enables greater use of immutable infrastructure. When using elasticity tools like auto-scale groups each production system is launched dynamically, based on a baseline image and may be automatically deprovisioned without human interaction. Thus, core operational requirements mean you never want to allow an administrator to log into a system and make changes, since they will be lost during a normal auto-scale activity. This enables the use of *immutable* servers where remote administration is completely disabled. We describe immutable servers and infrastructure in more detail in Domain 7.

* *DevOps.* DevOps is a new application development methodology and philosophy focused on automation of application development and deployment. DevOps opens up many opportunities for security to improve code hardening, change management and production application security, and even enhance security operations in general.

### Challenges

* *Limited visibility.* Visibility and the availability of monitoring and logging are impacted, requiring new approaches to gathering security-related data. This is especially true when using PaaS where commonly available logs, like system or network logs, are often no longer accessible. 

* *Increased application scope.* The management plane/metastructure security directly affects the security of any applications associated with that cloud account. Developers and operations will also likely need access to the management plane, as opposed to always going through a different team. Data and sensitive information is also potentially exposable within the management plane. Lastly, modern cloud applications often connect with the management plane to trigger a variety of automated actions, especially when PaaS is involved. For all those reasons management plane security is now within scope of the application's security and a failure on either side could bridge into the other. 

* *New threat models.* The cloud provider relationship and the shared security model will need to be included in the threat model as well as any operational and incident response plans.

* *Reduced transparency.* There may be less transparency as to what is going on within the application, especially as it integrates with external services. For example, you rarely know the entire set of security controls for an external PaaS service integrated with your application.

Overall, there will be changes due to the shared security model. Some of these are directly tied to governance and operations, but there are many more in terms of how you think and plan for the application's security.

## Overview

Due to the broad nature of application security and the many different skill sets and roles involved in an effective application security program, this domain is broken into the following major areas:

* *The Secure Software Development Lifecycle:* How cloud computing affects application security, from design to deployment.
* *Design and Architecture:* Trends in designing applications for cloud computing that affect and can even improve security.
* *DevOps and Continuous Integration/Continuous Deployment (CI/CD):* DevOps and CI/CD are very frequently used in both the development and deployment of cloud applications, and are becoming the dominant model. They bring new security considerations, and again, opportunities to improve security over more manual development and deployment patterns.

### Introduction to the Secure Software Development Lifecycle and Cloud Computing

The Secure Software Development Lifecycle (SSDLC) describes a series of security activities during all phases of application development, deployment, and operations. There are multiple frameworks used in the industry, including:

* Microsoft's Security Development Lifecycle
* NIST 800-64
* ISO/IEC 27034
* Other organizations, including OWASP and a variety of application security vendors, also publish their own lifecycle and security activities guidance. 

Due to the range of frameworks and differences in terminology, the Cloud Security Alliance breaks these into larger "meta-phases" to help describe the relatively standard set of activities seen across the frameworks. These aren't meant to replace the formal methodologies, but merely provide a descriptive model that we can use to address the major activities, independent of whatever lifecycle an organization will standardize on.
	
* *Secure Design and Development:* From training and developing organization-wide standards to actually writing and testing code.
* *Secure Deployment:* The security and testing activities when moving code from an isolated development environment into production.
* *Secure Operations:* Securing and maintaining production applications, including external defenses like Web Application Firewalls and ongoing vulnerability assessments.
	
Cloud computing affects every phase of the SSDLC, regardless of which particular SSDLC you use. This is a direct result of the abstraction and automation of cloud computing, combined with (in the public cloud) a greater reliance on an external provider. Specifically:

* The shared responsibility model means there is always some reliance on the cloud provider for some aspects of security, even in a very bare-bones IaaS-based application. The more you adopt PaaS and provider-specific features, the greater the split in security responsibilities. It could be as simple as using a cloud load balancer, which the provider is completely responsible for keeping secure but the cloud consumer is responsible for configuring and using properly.

* There are large changes in visibility and control, as discussed in nearly every domain of this Guidance.  When running mostly on IaaS it might just be a lack of network logs, but as you move into PaaS it may mean a loss of server and service logs. And it will all vary based on provider and technology.

* Different cloud providers have different capabilities in terms of features, services, and security, which must be accounted for in the overall application security plan.

* The management plane and metastructure may now be within the application security scope, especially when the application components communicate directly with the cloud service.

* There are new and different architectural options, especially, again, as you consume PaaS.

* The rise and impact of DevOps, which we cover later in this Domain.
		
### Secure Design and Development
			
There are five main phases in secure application design and development, all of which are affected by cloud computing:
				
* *Training:* Three different roles will require two new categories of training. Development, operations, and security should all receive additional training on cloud security fundamentals (which are not provider specific) as well as appropriate technical security training on any specific cloud providers and platforms used on their projects. There is typically greater developer and operations involvement in directly architecting and managing the cloud infrastructure, so baseline security training that's specific to the tools they will use is essential.

* *Define:* The organization determines the approved architectures or features/tools for the provider, security standards, and other requirements. This might be tightly coupled to compliance requirements, listing, for example, what kind of data is allowed onto which cloud services (including individual services within a larger provider). At this step the deployment processes should also be defined, although that is sometimes finalized later in a project. Security standards should include the initial entitlements for who is allowed to manage which services in the cloud provider, which is often independent of the actual application architecture. It should also include pre-approved tools, technologies, configurations, and even design patterns.

* *Design:* During the application design process, especially when PaaS is involved, the focus for security in cloud is on architecture, the cloud provider's baseline capabilities, cloud provider features, and automating and managing security for deployment and operations. We find that there are often significant security benefits to integrating security into the application architecture since there are opportunities to leverage the provider's own security capabilities. For example, inserting a serverless load balancer or message queue could completely block certain network attack paths. This is also where you perform threat modeling, which must also be cloud and provider/platform specific. 

* *Develop:* Developers may need a development environment with administrative access to the cloud management plane so that they can configure networks, services, and other settings. This should never be a production environment or hold production data. Developers will also likely use a CI/CD pipeline, which must be secured—especially the code repository. If PaaS is used, then developers should build logging into their application to compensate, as much as possible, for any loss of network, system, or service logs.

* *Test:* Security testing should be integrated into the deployment process and pipeline. Testing tends to span this and the *Secure Deployment* phase, but leans towards security unit tests, security functional tests, Static Application Security Testing (SAST), and Dynamic Application Security Testing (DAST). Due to the overlap we cover the cloud considerations in more depth in the next section. Organizations should also rely more on automated testing in cloud. Infrastructure is more often in scope for application testing due to "infrastructure as code" where the infrastructure itself is defined and implemented through templates and automation. As part of security testing, consider requiring flagging features for security-sensitive capabilities, such as authentication and encryption code, that may require deeper security review.
		
### Secure Deployment
			
Since deployment automation tends to be more prominent in cloud environments, it often includes certain security activities that could also be implemented in the Design and Development phase. Automated security testing is very frequently integrated into the deployment pipeline and performed outside of direct developer control. This is in and of itself a departure from many on-premises development efforts, but the testing itself also needs to be adapted for cloud computing.

There are multiple kinds of application security tests that could be potentially integrated into development and deployment:

* *Code Review:* This is a manual activity that's not necessarily integrated into automated testing, but the CI/CD pipeline might impose a manual gate. The review itself doesn't necessarily change for cloud but, there are specific areas that need additional attention. Any application communications with the management plane (e.g. API calls to the cloud service, some of which can alter the infrastructure) should be scrutinized, especially early in the project. Aside from looking at the code itself, the security team can focus on ensuring that only the least privilege entitlements are enabled for that part of the application and then validate them with the management plane configuration. Anything related to authentication and encryption is also important for additional review. The deployment process can then be automated to notify security if there are any modifications to these portions of code that might require manual approval, or just after-the-fact change review.
					
* *Unit testing, regression testing, and functional tests:* These are the standard tests used by developers in their normal processes. Security testing can and should be integrated in these to ensure that the security features in the application continue to function as expected. The tests themselves will likely need to be updated to account for running in the cloud, including any API calls. 

* *Static Application Security Testing (SAST):* On top of the normal range of tests, these should ideally incorporate checks on API calls to the cloud service. They should also look for any static embedded credentials for those API calls, which is a growing problem. 

* *Dynamic Application Security Testing (DAST):* DAST tests running applications and includes tests such as web vulnerability testing and fuzzing. Due to the terms of service with the cloud provider DAST may be limited and/or require pre-testing permission from the provider. With cloud and automated deployment pipelines it is possible to stand up entirely functional test environments using infrastructure as code and then perform deep assessments before approving changes for production. 

#### Impact on vulnerability assessment

Vulnerability assessment can be integrated into CI/CD pipelines and implemented in cloud fairly easily, but it nearly always requires compliance with the provider's terms of service. 

There are two specific patterns we commonly see. The first is running full assessments against images or containers as part of the pipeline in a special testing area of the cloud (a segment of a virtual network) that you define for this purpose. The image is only approved for production deployments if it passes this test. We see a similar pattern used to test entire infrastructures by building a test environment using infrastructure as code. 

In both cases production is tested less, or not at all, since it should be immutable and exactly resemble the test environment (both are based on the same definition files). Organizations can also use host-based vulnerability assessment tools, which run locally in a virtual machine and thus do not require coordination with or permission of the cloud provider.

#### Impact on penetration testing

As with vulnerability assessment there will almost certainly be limits on performing penetration tests without the permission of the cloud provider. The CSA recommends adapting penetration testing for cloud using the following guidelines:

* Use a testing firm that has experience on the cloud provider where the application is deployed.
* Include developers and cloud administrators within the scope of the test. Many cloud breaches attack those who maintain the cloud, not the application on the cloud itself.
* If the application is a multi-tenant app allow the penetration testers authorized access as a tenant to see if they can compromise the tenancy isolation and use their access to break into another tenant's environment or data.

#### Deployment pipeline security

CI/CD pipelines can enhance security through support of immutable infrastructure (fewer manual changes to production environments), automating security testing, and extensive logging of application and infrastructure changes when those changes run through the pipeline. When configured properly logs can track every code, infrastructure, and configuration change and tie them back to whoever submitted the change and whoever approved it, and they will also include any testing results. 

The pipeline itself needs to be tightly secured. Consider hosting pipelines in a dedicated cloud environment with very limited access to the cloud or the infrastructure hosting the pipeline components.

#### Impact of infrastructure as code and immutable

In multiple places we refer to *infrastructure as code*. Due to the virtual and software-defined nature of cloud it is often possible to define entire environments using templates that are translated by tools (either the provider's or third party) into API calls that automatically build the environment. A basic example is building a server configuration from a template. More complex implementations can build entire cloud application stacks, down to the network configuration and identity management. 

Since these environments are built automatically from a set of source file definitions they can also be *immutable*. If the system or environment is built automatically from a template, likely from a CI/CD pipeline, then any changes made in production will be overwritten by the next code or template change. The production environment can thus be locked down much more tightly than is normally possible in a non-cloud application deployment, where much of the infrastructure is configured manually to a specification. When security is properly engaged, the use of infrastructure as code and immutable deployments can significantly improve security.
		
### Secure Operations

When an application is deployed into production security activities continue. Many of these are covered in other areas throughout this guidance, especially in Domain 7 (Infrastructure), Domain 11 (Data), Domain 8 (Containers), and Domain 12 (Identity and Access Management). This section contains additional guidance that more directly applies to applications:

* The management plane for production environments should be much more tightly locked down than those for development. As previously mentioned, if the application directly accesses the management plane for the environment where it is hosted, then those privileges should be scoped to the least possible required. We recommend using multiple sets of credentials for each application service in order to further compartmentalize entitlements.

* Even when using immutable infrastructure, the production environment should still be actively monitored for changes and deviations from approved baselines. This can and should be automated through code (or tools) that make API calls to the cloud in order to regularly assess configuration state. 

On some cloud platforms it may be possible to use built-in assessment and configuration management features. It may also be possible to automatically remediate unapproved changes, depending on the platform and the nature of the change. For example, code can automatically revert any firewall rule changes that weren't approved by security.
			
* Even after deployment, and even using immutable infrastructure, don't neglect ongoing application testing and assessment. In public cloud scenarios, this will likely require coordination with or permission of the cloud provider to avoid violating terms of service, just like any other vulnerability assessment. 

* Change management doesn't just include the application, but also any infrastructure and the cloud management plane. 

For information on incident response see Domain 9, and for more on business continuity and management plane security see Domain 6.

### How cloud impacts application design and architectures

The very nature of cloud is already creating changes in preferred application designs, architectures, and patterns. Some of these have nothing directly to do with security, but the following trends offer opportunities to reduce common security issues:

* *Segregation by default:* Applications can easily be run in their own isolated cloud environments. Depending on the provider this could be a separate virtual network or account/sub-account. Using accounts or sub-account structures offers the benefit of enabling management plane segregation. The organization can open up wider rights in development accounts while running highly-restrictive production accounts.  

* *Immutable infrastructure:* As mentioned, immutable infrastructure is becoming increasingly common in cloud, for operational reasons. Security can extend these benefits by disabling remote logins to immutable servers/containers, adding file integrity monitoring, and integrating immutable into incident recovery plans. 

* *Increased use of micro-services:* In cloud computing, it is easier to segregate out different services onto different servers (or containers), since you no longer need to maximize utilization of physical servers and auto-scale groups can assure application scalability even when using fleets of smaller computer nodes for workloads. Since each node does less, it's easier to lock down and minimize the services running on it. While this improves the security of each workload (when used correctly), it does add some overhead to secure the communications between all the microservices and ensure that any service discovery, scheduling, and routing is also configured securely.

* *PaaS and "serverless" architectures:* With PaaS and "serverless" setups (running workloads directly on the cloud provider's platform where you don't manage the underlying services and operating system) there is great potential for dramatically reducing the attack surface. This is only if the cloud provider takes responsibility for the security of the platform/serverless and meets your requirements. 

Serverless can bring a few advantages. First, there are large economic incentives for the provider to maintain extremely high security levels and keep their environment up to date. This removes the day-to-day responsibility for keeping these secure from the cloud consumer, but never obviates their ultimate accountability for security. Working with a trusted cloud provider with a strong track record is critical. 

Next, the serverless platforms may run on the provider's network with communications to the consumer's components through API or HTTPS traffic. This removes direct network attack paths, even if an attacker compromises a server or container. The attacker is limited to attempting API calls or HTTPS traffic and can't port scan, identify other servers, or use other common techniques. 

* *Software defined security:* Security teams can leverage all the same tools and technologies to automate many security operations, even integrating them with the application stack. Some examples including automating cloud incident response, automating dynamic changes to entitlements, and remediation of unapproved infrastructure changes. 

* *Event driven security:* Certain cloud providers support event-driven code execution. In these cases, the management plane detects various activities, such as a file being uploaded to a designated object storage location or configuration changes to the network or identity management, which can in turn trigger code execution through a notification message or via serverless hosted code. Security can define events for security actions and use the event-driven capabilities to trigger automated notification, assessment, remediation, or other security processes. 

### Additional considerations for cloud providers

Cloud providers of all service models need to pay extra attention to certain aspects of their application services that could cause very significant problems for their customers if there are security issues:

* APIs and web services need to be extensively hardened and assume attacks from both authenticated and unauthenticated adversaries. This includes using industry-standard authentication designed specifically for APIs.
* APIs should be monitored for abuse and unusual activity. 
* The service should undergo extensive design and testing to prevent attacks or inappropriate/accidental cross-tenant access.

### The rise and role of DevOps

DevOps refers to the deeper integration of development and operations teams through better collaboration and communications, with a heavy focus on automating application deployment and infrastructure operations. There are multiple definitions, but the overall idea consists of a culture, philosophy, processes, and tools. 

At the core is the combination of Continuous Integration and/or Continuous Delivery (CI/CD) through automated deployment pipelines, and the use of programmatic automation tools to better manage infrastructure. DevOps is not exclusive to cloud, but as discussed it is highly attuned to cloud and is growing to become the dominant cloud application delivery model.

#### Security implications and advantages

* *Standardization:* With DevOps, anything that goes into production is created by the CI/CD pipeline on approved code and configuration templates. Dev/Test/Prod are all based on the exact same source files, which eliminates any deviation from known good standards.

* *Automated testing:* As discussed, a wide variety of security testing can be integrated into the CI/CD pipeline, with manual testing added as needed to supplement.

* *Immutable:* CI/CD pipelines can produce master images for virtual machines, containers, and infrastructure stacks very quickly and reliably. This enables automated deployments and immutable infrastructure.

* *Improved auditing and change management:* CI/CD pipelines can track everything, down to individual character changes in source files that are tied to the person submitting the change, with the entire history of the application stack (including infrastructure) stored in a version control repository. This offers considerable audit and change-tracking benefits.

* *SecDevOps/DevSecOps and Rugged DevOps*: These two terms are emerging to describe the integration of security activities into DevOps. SecDevOps/DevSecOps sometimes refers to the use of DevOps automation techniques to improve security operations. Rugged DevOps refers to integration of security testing into the application development process to produce harder, more secure, and more resilient applications.

## Recommendations

* Understand the security capabilities of your cloud providers. Not merely their baseline, but the various platforms and services.
* Build security into the initial design process. Cloud deployments are more-often greenfield, creating new opportunities to engage security early.
* Even if you don't have a formal SDLC, consider moving to continuous deployment and automating security into the deployment pipeline.
* Threat modeling, SAST, and DAST (with fuzzing) should all be integrated. Testing should be configured to work in the cloud environment, but also to test for concerns *specific* to cloud platforms, such as stored API credentials.
* Understand the new architectural options and requirements in the cloud. Update your security policies and standards to support them, and don't merely attempt to enforce existing standards on an entirely different computing model.
* Integrate security testing into the deployment process.
* Use software defined security to automate security controls.
* Use event-driven security, when available, to automate detection and remediation of security issues.
* Use different cloud environments to better segregate management plane access and provide developers the freedom they need to configure development environments, while also locking down production environments.