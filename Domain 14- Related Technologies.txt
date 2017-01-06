# Domain 14: Related Technologies

## Introduction

Throughout this Guidance we have focused on providing background information and best practices for directly securing cloud computing. As such a foundational technology there are also a variety of related technologies that bring their own particular security concerns. While covering all potential uses of cloud is well beyond the scope of this document the Cloud Security Alliance feels it is important to include background and recommendations for key technologies that are interrelated with cloud. Some, such as containers and Software Defined Networks, are so tightly intertwined we cover them in other respective domains of the Guidance. This Domain provides more depth on additional technologies that don't fit cleanly into existing domains.

Breaking these out into their own section provides more flexibility to update coverage; adding and removing technologies as their usage shifts and new capabilities emerge.

## Overview

Related technologies fall into two broad categories:

* Technologies that rely nearly exclusively on cloud computing to operate. 
* Technologies that don't necessarily rely on cloud, but are commonly seen in cloud deployments.

That isn't to say these technologies *can't* work without cloud, just that they are extremely commonly seen overlapping or relying with cloud deployments and are so commonly seen that they have implications for the majority of cloud security professionals.

The current list includes:

* Big Data
* Internet of Things (IoT)
* Mobile devices

Each of these technologies is currently covered by additional Cloud Security Alliance research working groups in multiple, ongoing projects and publications:

* [Big Data Working Group](https://cloudsecurityalliance.org/group/big-data/)
* [Internet of Things Working Group](https://cloudsecurityalliance.org/group/internet-of-things/)
* [Mobile Working Group](https://cloudsecurityalliance.org/group/mobile/)

### Big Data

* Big data includes a collection of technologies for working with extremely large datasets that traditional data processing tools are unable to manage. It is not any single technology but commonly refers to distributed collection, storage, and data processing frameworks.
* Gartner defines it as ["Big data is high volume, high velocity, and/or high variety information assets that require new forms of processing to enable enhanced decision making, insight discovery and process optimization."](http://www.gartner.com/newsroom/id/1731916)
	* The "3 Vs" are commonly accepted as the core definition of big data, although there are many other interpretations.
* Cloud Computing, due to its elasticity and massive storage capabilities, is very often where big data projects are deployed.
	* Big data is not exclusive to cloud.
	* Big data technologies are very commonly integrated into cloud computing applications and offered by cloud providers as IaaS or PaaS.
* There are three common components, regardless of the specific toolset used:
	* Distributed data collection: Mechanisms to ingest large volumes of data, often of a streaming nature. This could be as "lightweight" as web click streaming analytics and as complex as highly distributed scientific imaging or sensor data. 
		* Not all Big Data relies on distributed or streaming data collection, but it is a core Big Data technology.
	* Distributed storage: the ability to store the large data sets in distributed file systems or databases which is often required due to the limitations of non-distributed storage technologies.
	* Distributed processing for the effective analysis of such massive and rapidly changing data sets that single-origin processing can't effectively handle.
* Security considerations
	* Data collection mechanisms will likely use intermediary storage that needs to be appropriately secured.
		* E.g. if run in containers or virtual machines ensure the underlying storage is appropriately secured.
	* Key management for storage may be complicated depending on the exact mechanisms used.
		* There are techniques to properly encrypt most big data storage layers today, and these align with our guidance in *Domain 11- Data Security and Encryption*. The complicating factor is that key management needs to handle distributing keys to multiple storage and analysis nodes.
	* Not all big data technologies have robust security capabilities. In some cases cloud provider security capabilities can help compensate for the big data technology limitations. Both should be included in any security architecture.
	* Distributed analysis/processing nodes will also likely use some form of intermediate storage that will need additional security.
	* Identity and Access Management will likely occur at both cloud and big data tool levels, which can complicate entitlement matrices. 
	* Many cloud providers are expanding big data support with *machine learning* and other platform as a service options that rely on access to enterprise data. These should not be used without a full understanding of potential data exposure, compliance, and privacy implications. 
		* This doesn't mean you shouldn't use the services, it means you need to understand the implications and make an appropriate risk decisions. Machine learning and other analysis services aren't necessarily insecure and don't necessarily violate privacy and compliance commitments.

### Internet of Things (IoT)

The Internet of Things is a blanket term for non-traditional computing devices used in the physical world that utilize Internet connectivity. It includes everything from Internet-enabled operational technology (used by utilities like power and water) to fitness trackers, connected lightbulbs, medical devices, and beyond.

A very large percentage of these devices connect back to cloud computing infrastructure for their back end processing and data storage. Key cloud security issues related to IoT include:

* Secure data collection and sanitization.
* Device registration, authentication, and authorization.
	* Eliminating the use of stored credentials that could expose the cloud-side application.
* API security for connections from devices back to the cloud infrastructure.
* Encrypted communications.
* Ability to patch and update devices so they don't become a point of compromise.

### Mobile

* A very large percentage of mobile applications connect to cloud computing for their back end processing. This section won't discuss overall mobile security but just on the portions that affect cloud security.
* The primary security issues are very similar to IoT, except a mobile phone or tablet is also a general purpose computer:
	* Device registration, authentication, and authorization is a common source of issues. Especially the use of stored credentials, and especially when the mobile device connects directly to the cloud provider's infrastructure/APIs. Attackers have been known to decompile mobile applications to reveal stored credentials which are then used to directly manipulate/attack the cloud infrastructure.
		* Data stored on the device should also be protected with the assumption that the user of the device may be a hostile attacker.
	* Application APIs are also a potential source of compromise. Attackers are known to sniff API connections, in some cases using local proxies they redirect their own devices towards, and then decompile the (likely now unencrypted) API calls and explore for security weaknesses.
		* Certificate pinning/validation inside the device application may help reduce this risk. 

	
#Recommendations

* Big data
	* Leverage cloud provider capabilities wherever possible, even if they overlap with big data tool security capabilities. This ensures you have proper protection within the cloud metastructure and the specific application stack.
	* Use encryption for primary, intermediary, and backup storage for both data collection and data storage planes.
	* Include both the big data tool and cloud platform Identity and Access Management in the project entitlement matrix.
	* Fully understand the potential benefits and risks of using a cloud machine learning or analytics service. Pay particular attention to privacy and compliance implications. 
		* Cloud providers should assure customer data is not exposed to employees or other administrators using technical and process controls.
		* Cloud providers should clearly publish which compliance standards their analytics and machine learning services are compliant with (for their customers).
		* Cloud consumers should consider use of data masking or obfuscation when considering a service that doesn't meet security, privacy, or compliance requirements. 
	* Follow additional big data security best practices, including those provided by the tool vendor (or Open Source project) and [the Cloud Security Alliance](https://downloads.cloudsecurityalliance.org/assets/research/big-data/BigData_Security_and_Privacy_Handbook.pdf).
* Internet of Things
	* Ensure devices can be patched and upgraded.
	* Do not store static credentials on devices that could lead to compromise of the cloud application or infrastructure.
	* Follow best practices for secure device registration and authentication to the cloud-side application, typically using a federated identity standard.
	* Encrypt communications.
	* Use a secure data collection pipeline and sanitize data to prevent exploitation of the cloud application or infrastructure through attacks on the data collection pipeline.
	* Assume all API requests are hostile.
	* Follow the additional, more-detailed guidance issued by the [CSA Internet of Things working group](https://cloudsecurityalliance.org/group/internet-of-things/).
* Mobile
	* Follow your cloud provider's guidance on properly authenticating and authorizing mobile devices when designing an application that connects directly to the cloud infrastructure.
	* Use industry standards, typically federated identity, for connecting mobile device applications to cloud-hosted applications.
	* Test all APIs under the assumption that a hostile attacker will have authenticated, unencrypted access.
		* Consider certificate pinning and validation inside mobile applications.
		* Validate all API data and sanitize for security.
		* Implement server/cloud-side security monitoring for hostile API activity.
	* Ensure all data stored on device is secured and encrypted. 
		* Sensitive data that could allow compromise of the application stack should not be stored locally on-device where a hostile user can potentially access it.
	* Follow the more-detailed recommendations and research issued by the [CSA Mobile working group](https://cloudsecurityalliance.org/group/mobile/).





