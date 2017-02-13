# Domain 13: Security as a Service

## Introduction

While most of this Guidance focuses on securing cloud platforms and deployments, this domain shifts direction to cover security services delivered *from* the cloud. These services, which are typically SaaS or PaaS, aren't necessarily used exclusively to protect cloud deployments; they are just as likely to help defend traditional on-premises infrastructure.

Security as a Service (SecaaS) providers offer security capabilities *as a cloud service*. This includes dedicated SecaaS providers as well as packaged security features from general cloud-computing providers. Security as a Service encompasses a very wide range of possible technologies, but they must meet the following criteria:

* SecaaS includes security products or services that are delivered as a cloud service.
* To be considered SecaaS, the services must still meet the essential NIST characteristics for cloud computing, as defined in Domain 1.

This section highlights some of the more common categories in the market, but SecaaS is constantly evolving and the descriptions and following list should not be considered canonical.

## Overview

### Potential benefits and concerns of SecaaS
 
Before delving into the details of the different significant SecaaS categories it is important to understand how SecaaS is different from both on-premises and self-managed security. To do so, consider the potential benefits and consequences.
	
#### Potential benefits

* *Cloud-computing benefits.* The normal potential benefits benefits of cloud computing—such as reduced capital expenses, agility, redundancy, high-availability, and resiliency—all apply to SecaaS. As with any other cloud provider the magnitude of these benefits depend on the pricing, execution, and capabilities of the security provider.

* *Staffing and expertise.* Many organizations struggle to employ, train, and retain security professionals across relevant domains of expertise. This can be exacerbated due to limitations of local markets, high costs for specialists, and balancing day-to-day needs with the high rate of attacker innovation. As such, SecaaS providers bring the benefit of extensive domain knowledge and research that may be unattainable for many organizations who are not solely focused on security or the specific security domain.

* *Intelligence sharing.* SecaaS providers protect multiple clients simultaneously and have the opportunity to share data intelligence and data across them. For example, finding a malware sample in one client allows the provider to immediately add it to their defensive platform, thus protecting all other customers. Practically speaking this isn't a magic wand, as the effectiveness will vary across categories, but since intelligence sharing is built into the service, the potential upside is there. 

* *Deployment flexibility.* SecaaS may be better positioned to support evolving workplaces and cloud migrations, since it is itself a cloud-native model delivered using broad network access and elasticity. Services can typically handle more-flexible deployment models, such as supporting distributed locations without the complexity of multi-site hardware installations.

* *Insulation of clients.* In some cases, SecaaS can intercept attacks before they hit the organization directly. For example, spam filtering and cloud-based Web Application Firewalls are positioned *between* the attackers and the organization. They can absorb certain attacks before they ever reach the customer's assets.
		
* *Scaling and cost.* The cloud model provides the consumer with a “Pay as you Grow” model, which also helps organizations focus on their core business and lets them leave security concerns to the experts.
	
#### Potential concerns

* *Lack of visibility.* Since services operate at a remove from the customer, they often provide less visibility or data compared to running one's own operation. The SecaaS provider may not reveal details of how it implements its own security and manages its own environment. Depending on the service and the provider, that may result in a difference in data sources and the level of detail available for things like monitoring and incidents. Some information that the customer may be accustomed to having may look different, have gaps, or not be available at all. The actual evidence and artifacts of compliance, as well as other investigative data, may not meet the customer's goals. All of this can and should be determined before entering into any agreement.

* *Regulation differences.* Given global regulatory requirements, SecaaS providers may be unable to assure compliance in all jurisdictions that an organization operates in. 

* *Handling of regulated data.* Customers will also need assurance that any regulated data potentially vacuumed up as part of routine security scanning or a security incident is handled in accordance with any compliance requirements; this also needs to comply with aforementioned international jurisdictional differences. For example, employee monitoring in Europe is more restrictive than it is in the United States, and even basic security monitoring practices could violate workers' rights in that region. Likewise, if a SecaaS provider relocates its operations, due to data center migration or load balancing, it may violate regulations that have geographical restrictions in data residence.

* *Data leakage.* As with any cloud computing service or product, there is always the concern of data from one cloud consumer leaking to another. This risk isn't unique to SecaaS, but the highly sensitive nature of security data (and other regulated data potentially exposed in security scanning or incidents) does mean that SecaaS providers should be held to the highest standards of multi-tenant isolation and segregation. Security-related data is also likely to be involved in litigation, law enforcement investigations, and other discovery situations. Customers want to ensure their data will not be exposed when these situations involve another client on the service. 

* *Changing providers.* Although simply switching SecaaS providers may on the surface seem easier than swapping out on-premises hardware and software, organizations may be concerned about lock-in due to potentially losing access to data, including historical data needed for compliance or investigative support.

* *Migration to SecaaS.* For organizations that have existing security operations and on-premises legacy security control solutions, the migration to SecaaS and the boundary and interface between any in-house IT department and SecaaS providers must be well planned, exercised, and maintained.

### Major Categories of Security as a Service Offerings
There are a large number of products and services that fall under the heading of Security as a Service. While the following is not a canonical list, it describes many of the more common categories seen as of this writing:
		
#### Identity, Entitlement, and Access Management Services
			 
Identity-as-a-service is a generic term that covers one or many of the services that may comprise an identity ecosystem, such as Policy Enforcement Points (PEP-as-a-service), Policy Decision Points (PDP-as-a-service), Policy Access Points (PAP-as-a-service), services that provide Entities with Identity, services that provide attributes (e.g. Multi Factor Authentication), and services that provide reputation. 

One of the more well-known categories heavily used in cloud security are Federated Identity Brokers. These services help intermediate IAM between an organization's existing identity providers (internal or cloud-hosted directories) and the many different cloud services used by the organization. They can provide web Single Sign On (SSO), helping ease some of the complexity of connecting to a wide range of external services that use different federation configurations.

There are two other categories commonly seen in cloud deployments: Strong authentication services use apps and infrastructure to simplify the integration of various strong authentication options, including mobile device apps and tokens for MFA. The other category hosts directory servers in the cloud to serve as an organization's identity provider.

#### Cloud Access and Security Brokers (CASB, also known as Cloud Security Gateways)

These products intercept communications that are directed towards a cloud service, or directly connect to the service via API, in order to monitor activity, enforce policy, and detect and/or prevent security issues. They are most commonly used to manage an organization's sanctioned and unsanctioned SaaS services. While there are on-premises CASB options, it is also often offered as a cloud-hosted service.

CASBs can also connect to on-premises tools to help an organization detect, assess, and potentially block cloud usage and unapproved services. Many of these tools include risk-rating capabilities to help customers understand and categorize hundreds or thousands of cloud services. The ratings are based on a combination of the provider's assessments, which can be weighted and combined with the organization's priorities.

Most providers also offer basic Data Loss Prevention for the covered cloud services, inherently or through partnership and integration with other services. 

Depending on the organization discussing "CASB," the term is also sometimes used to include Federated Identity Brokers. This can be confusing: although the combination of the "security gateway" and "identity brokers" capabilities is possible and does exist, the market is still dominated by independent services for those two capabilities. 

#### Web Security (Web Security Gateways)

Web Security involves real-time protection, offered either on-premises through software and/or appliance installation, or via the Cloud by proxying or redirecting web traffic to the cloud provider (or a hybrid of both). This provides an added layer of protection on top of other protection, such as anti-malware software to prevent malware from entering the enterprise via activities such as web browsing. In addition, it can also enforce policy rules around types of web access and the time frames when they are allowed. Application authorization management can provide an extra level of granular and contextual security enforcement for web applications.  

#### Email Security 

Email Security should provide control over inbound and outbound email, protecting the organization from risks like phishing and malicious attachments, as well as enforcing corporate polices, such as acceptable use and spam prevention, and providing business continuity options. 

In addition, the solution may support policy-based encryption of emails as well as integrating with various email server solutions. Many email security solutions also offer features like digital signatures that enable identification and non-repudiation. This category includes the full range of services, from those as simple as anti-spam features all the way to fully-integrated email security gateways with advanced malware and phishing protection.

#### Security Assessment

Security assessments are third-party or customer-driven audits of cloud services or assessments of on-premises systems via cloud-provided solutions. Traditional security assessments for infrastructure, applications, and compliance audits are well defined and supported by multiple standards such as NIST, ISO, and CIS. A relatively mature toolset exists, and a number of tools have been implemented using the SecaaS delivery model. Using that model, subscribers get the typical benefits of cloud-computing: variant—elasticity, negligible setup time, low administration overhead, and pay-per-use with low initial investments. 

There are three main categories of security assessments:

* Traditional security/vulnerability assessments of assets that are deployed in the cloud (e.g. virtual machines/instances for patches and vulnerabilities) or on-premises.
* Application security assessments, including SAST, DAST, and management of RASP.
* Cloud platform assessment tools that connect directly with the cloud service over API to assess not merely the assets deployed in the cloud, but the cloud configuration as well.

#### Web Application Firewalls

In a cloud-based Web Application Firewall (WAF), customers redirect traffic (using DNS) to a service that analyzes and filters traffic before passing it through to the destination web application. Many cloud WAFs also include anti-DDoS capabilities.
		
#### Intrusion Detection/Prevention (IDS/IPS) 

Intrusion Detection/Prevention systems monitor behavior patterns using rule-based, heuristic, or behavioral models to detect anomalies in activity which might present risks to the enterprise. With IDS/IPS as a service, the information feeds into a service-provider's managed platform, as opposed to the customer being responsible for analyzing events themselves. Cloud IDS/IPS can use existing hardware for on-premises security, virtual appliances for in-cloud (see Domain 7 for the limitations), or host-based agents.

#### Security Information & Event Management (SIEM) 

Security Information and Event Management (SIEM) systems aggregate (via push or pull mechanisms) log and event data from virtual and real networks, applications, and systems. This information is then correlated and analyzed to provide real-time reporting on and alerting of information or events that may require intervention or other types of responses. Cloud SIEMs collect this data in a cloud service, as opposed to a customer-managed, on-premises system.

#### Encryption and Key Management

These services encrypt data and/or manage encryption keys. They may be offered by cloud services to support customer-managed encryption and data security. They may be limited to only protecting assets within that specific cloud provider, or they may be accessible across multiple providers (and even on-premises, via API) for broader encryption management. The category also includes encryption proxies for SaaS, which intercept SaaS traffic to encrypt discrete data.

However, encrypting data *outside* a SaaS platform may affect the ability of the platform to utilize the data in question.

#### Business Continuity and Disaster Recovery 

Providers of cloud BC/DR services back up data from individual systems, data centers, or cloud services to a cloud platform instead of relying on local storage or shipping tapes. They may use a local gateway to speed up data transfers and local recoveries, with the cloud service serving as the final repository for worst-case scenarios or archival purposes.

#### Security management

These services roll up traditional security management capabilities, such as EPP (endpoint) protection agent management, network security, mobile device management, and son into a single cloud service. This reduces or eliminates the need for local management servers and may be particularly well suited for distributed organizations.

#### Distributed Denial of Service Protection

By nature, most DDoS protections are cloud-based. They operate by rerouting traffic through the DDoS service in order to absorb attacks before they can affect the customer's own infrastructure. 

## Recommendations

* Before engaging a SecaaS provider, be sure to understand any security-specific requirements for data handling (and availability), investigative, and compliance support.
* Pay particular attention to handling of regulated data, like PII.
* Understand your data retention needs and select a provider that can support data feeds that don't create a lock-in situation.
* Ensure that the SecaaS service is compatible with your current and future plans, such as its supported cloud (and on-premises) platforms, the workstation and mobile operating systems it accommodates, and so on.