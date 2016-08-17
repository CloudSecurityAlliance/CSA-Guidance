# Domain 13: Security as a Service

*Status note: certain sections of the Guidance V3 of this domain are directly included when they are still relevant, but new or altered content is included as an outline, not complete text. Please do not submit grammar/spelling issues, right now we are focused on content feedback. Thank you...*

## Introduction

* Security as a Service (SecaaS) providers offer security capabilities as a cloud service.
	* Are typically SaaS or PaaS.
	* Not limited to dedicated SecaaS providers, can include packaged security features from generalized cloud providers.
* SecaaS includes a very wide range of possibilities.
	* Essentially, any security product that is delivered as a cloud service.
	* To be considered SecaaS, the services must still meet the NIST essential characteristics.
	* This section will highlight some of the more-common categories in the market, but SecaaS is constantly evolving and the descriptions and list shouldn't be considered canonical.

## Overview

* Potential benefits and concerns
	* Potential benefits
		* The normal cloud benefits of reduced capital expenses, agility, resiliency, etc all still apply.
		* Many organizations struggle to maintain staffing of security professionals across relevant domains of expertise. SecaaS providers bring the benefit of extensive domain knowledge and research  for their category of service that may be unattainable for many organizations not solely focused on security.
		* SecaaS providers protect multiple clients simultaneously, and have the opportunity to share data intelligence and data across them. For example, finding one malware sample in one client can then immediately be added to a defensive platform to protect all other customers. Practically speaking this isn't a magic wand, and the effectiveness will vary across categories.
		* SecaaS may be better positioned to support evolving workplaces and cloud migrations. Services can typically meet more-flexible deployment models, such as supporting distributed locations without the complexity of multi-site hardware installations.
		* In some cases, SecaaS can intercept security attacks before they hit the organization directly.
	* Potential concerns
		* Lack of visibility and data. 
			* How the cloud provider actually implements security and manages their own environment.
			* Security data and incidents for forensics and analysis. Availability will vary across providers, and some won't necessarily provide the level of detail the customer may desire.
			* Customers may desire artifacts of compliance (logs, etc.) or investigative data beyond that provided by the service.
		* Compliance
			* Given global regulatory requirements, SecaaS providers may be unable to assure compliance in all jurisdictions an organization operates in.
			* Customers will need assurance that regulated data that is potentially vacuumed up as part of security scanning or incidents is handled in accordance with any compliance requirements. 
		* Multi-tenancy
			* As with any cloud computing, there is always the concern of data from one cloud consumer leaking to another. This isn't a unique risk to SecaaS, but the highly-sensitive nature of security data (and other regulated data potentially exposed in security scanning or incidents) does mean SecaaS providers should be held to the highest standards of isolation and segregation.
			* Security related data is also likely to be involved in litigation, law enforcement investigations, and other discovery situations. Customers want to ensure their data will not be exposed when these situations involve another client on the service. 
		* Lock-In
			* Although switching providers may be easier on the surface than swapping out on-premise hardware and software, organizations may be concerned about loss of access to data, including historical data needed for compliance or investigative support.
	* Major Categories of Security as a Service Offerings
		* This is not a canonical list, but describes the more-common categories we see today:
		* Identity, Entitlement, and Access Management Services
			* Identity-as-a-service is a generic term that covers one or many of the services that may comprise an identity eco-system, such as Policy Enforcement Points (PEP-as-a-service), Policy Decision Points (PDP-as-a-service), Policy Access Points (PAP-as-a-service), services that provide Entities with Identity, services that provide attributes (e.g. Multi Factor Authentication), and services that provide reputation.
			* These Identity services should provide controls for identities, access, and privileges management.  Identity services need to include people, processes, and systems that are used to manage access to enterprise resources by assuring the identity of an entity is verified, then granting the correct level of access based on this assured identity.  Audit logs of activities such as successful and failed authentication and access attempts should be managed by the application / solution or the SIEM service.  Identity, Entitlement, and Access Management services are a Protective and Preventative technical control.
		* Cloud Access and Security Brokers (CASB, also known as Cloud Security Gateways)
			* These products intercept communications directed towards a cloud service, or directly connect to the service via API, to monitor activity, enforce policy, and detect and/or prevent security issues.
			* They also connect to on-premise tools to help an organization detect, assess, and potentially block cloud usage and unapproved services.
				* Most tools include risk assessment capabilities to help customers understand and categorize hundreds or thousands of cloud services.
			* Most providers also offer Data Loss Prevention for the covered cloud services, inherently or through partnership.
		* Web Security (Web Security Gateways)
			* Web Security is real-time protection offered either on premise through software / appliance installation or via the Cloud by proxying or redirecting web traffic to the cloud provider.  This provides an added layer of protection on top of other protection such as anti-malware software to prevent malware from entering the enterprise via activities such as web browsing.  Policy rules around types of web access and the time frames when this is allowed can also be enforced via these technologies. Application authorization management can be used to provide an extra level of granular and contextual security enforcement for web applications.  
		* Email Security 
			* Email Security should provide control over inbound and outbound email, protecting the organization from phishing, malicious attachments, enforcing corporate polices such as acceptable use and spam prevention, and providing business continuity options.  In addition, the solution should allow for policy-based encryption of emails as well as integrating with various email server solutions.  Digital signatures enabling identification and non-repudiation are also features of many email security solutions.  The Email Security offering is a protective, detective, and reactive technical control.
		* Security Assessment
			* Security assessments are third party or customer-driven audits of cloud services or assessments of on premises systems via cloud provided solutions based on industry standards.  Traditional security assessments for infrastructure, applications and compliance audits are well defined and supported by multiple standards such as NIST, ISO, and CIS.  A relatively mature toolset exists, and a number of tools have been implemented using the SecaaS delivery model.  In the SecaaS delivery model, subscribers get the typical benefits of this cloud-computing variant—elasticity, negligible setup time, low administration overhead, and pay per use with low initial investments. 
			* Three main categories:
				* Traditional security/vulnerability assessments of assets deployed in the cloud (e.g. virtual machines/instances for patches and vulnerabilities).
				* Application security assessments
					* Can include both dynamic and static assessment as a service.
				* Cloud platform assessment
					* Tools that connect directly with the cloud service over API to assess the cloud configuration, not merely the assets deployed in the cloud.
		* Web Application Firewalls
			* Customers redirect traffic (using DNS) to a cloud-based WAF service that analyzes and filters traffic before passing it through to the destination web application.
			* Many also include DDoS capabilities.
		* Intrusion Detection/Prevention (IDS/IPS) 
			* Intrusion Detection/Prevention systems monitor behavior patterns using rule-based, heuristic, or behavioral models to detect anomalies in activity that present risks to the enterprise.  Network IDS/IPS have become widely used over the past decade because of the capability to provide a granular view of what is happening within an enterprise network.  The IDS/IPS monitors network traffic and compares the activity to a baseline via rule-based engine or statistical analysis.  IDS is typically deployed in a passive mode to passively monitor sensitive segments of a client’s network whereas the IPS is configured to play an active role in the defense of the clients network.  In a traditional infrastructure, this could include De-Militarized Zones (DMZ’s) segmented by firewalls or routers where corporate Web servers are locate or monitoring connections to an internal database. 
			* With IDS/IPS as a service the information feeds into a service-provider's managed platform, as opposed to the customer being responsible for analyzing events themselves.
		* Security Information & Event Management (SIEM) 
			* Security Information and Event Management (SIEM) systems aggregate (via push or pull mechanisms) log and event data from virtual and real networks, applications, and systems. This information is then correlated and analyzed to provide real time reporting and alerting on information or events that may require intervention or other types of responses.  The logs are typically collected and archived in a manner that prevents tampering to enable their use as evidence in any investigations or historical reporting. 
			* Cloud SIEMs collect this data in a cloud service as opposed to a customer-managed, on-premise system.
		* Encryption and Key Management
			* Services that encrypt data and/or manage encryption keys. 
			* Beginning to be offered by major cloud services to support customer-managed encryption and data security.
			* May be limited to only protecting assets within that cloud provider, or may be accessible across multiple providers (and even on-premise, via API) for broader encryption management.
			* Also includes encryption proxies for SaaS, which intercept SaaS traffic to encrypt discrete data.
				* Note that encrypting data outside a SaaS platform may affect the ability of the platform to utilize the data.
		* Business Continuity and Disaster Recovery 
			* Backing up data from individual systems, data centers, or cloud services to a cloud platform instead of relying on local storage or shipping tapes.
			* May use a local gateway to speed up data transfers and local recoveries, with the cloud service serving as the final repository for worst-case scenarios or archival purposes.
		* Security management
			* These services roll up traditional security management capabilities, such as EPP (endpoint) protection agent management, network security, mobile device management, etc. into a cloud service.
			* This reduces or eliminates the need for local management servers and may be particularly well suited for distributed organizations.
		* Distributed Denial of Service Protection
			* By nature, most DDoS protections are cloud based; rerouting traffic through the DDoS service to absorb attacks.

## Recommendations
	* Before engaging a SecaaS provider, understand any security-specific requirements for data handling (and availability), investigative, and compliance support.
		* Pay particular attention to handling of regulated data, like PII.
	* Understand your data retention needs and select a provider that can support data feeds that don't create a lock-in situation.
	* Ensure the SecaaS service is compatible with current and future plans, such as supported cloud (and on-premise) platforms, workstation and mobile operating systems, etc.