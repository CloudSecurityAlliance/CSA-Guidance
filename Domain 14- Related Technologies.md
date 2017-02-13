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
* Serverless computing

Each of these technologies is currently covered by additional Cloud Security Alliance research working groups in multiple, ongoing projects and publications:

* [Big Data Working Group](https://cloudsecurityalliance.org/group/big-data/)
* [Internet of Things Working Group](https://cloudsecurityalliance.org/group/internet-of-things/)
* [Mobile Working Group](https://cloudsecurityalliance.org/group/mobile/)

### Big Data

*Big data includes a collection of technologies for working with extremely large datasets that traditional data processing tools are unable to manage. It is not any single technology but commonly refers to distributed collection, storage, and data processing frameworks.

Gartner defines it as ["Big data is high volume, high velocity, and/or high variety information assets that require new forms of processing to enable enhanced decision making, insight discovery and process optimization."](http://www.gartner.com/newsroom/id/1731916)

The "3 Vs" are commonly accepted as the core definition of big data, although there are many other interpretations:
	* *High volume:* a large size of data, in terms of number of records or attributes.
	* *High velocity:* fast generation and processing of data, i.e., real-time or stream data.
	* *High variety:* structured, semi-structured, or unstructured data.

 Cloud Computing, due to its elasticity and massive storage capabilities, is very often where big data projects are deployed. Big data is not exclusive to cloud by any means, but  big data technologies are very commonly integrated into cloud computing applications and offered by cloud providers as IaaS or PaaS.
 
There are three common components, regardless of the specific toolset used:
	
	* *Distributed data collection:* Mechanisms to ingest large volumes of data, often of a streaming nature. This could be as "lightweight" as web click streaming analytics and as complex as highly distributed scientific imaging or sensor data. Not all Big Data relies on distributed or streaming data collection, but it is a core Big Data technology.
	* *Distributed storage*: The ability to store the large data sets in distributed file systems (such as, Google file system, hadoop distributed file system, etc.) or databases (often NoSQL) which is often required due to the limitations of non-distributed storage technologies.
	* *Distributed processing:* Tools capable of distributing processing jobs (such as, map reduce, spark, etc.) for the effective analysis of such massive and rapidly changing data sets that single-origin processing can't effectively handle.

#### Security and privacy considerations
	
	Due to a combination of the highly distributed nature of big data applications (with data collection, storage, and processing all distributed among diverse nodes) and the sheer volume and potential sensitivity of the information, security and privacy are typically high priorities but challenged by a patchwork of different tools and platforms. 
	
	* Data collection mechanisms will likely use intermediary storage that needs to be appropriately secured. This storage is used as part of the transfer of data from collection to storage. Even if primary storage is well-secured it's important to also check intermediary storage which might be as simple as some swap space on a processing node.
		* For example, if collection run in containers or virtual machines ensure the underlying storage is appropriately secured.
		* 	Distributed analysis/processing nodes will also likely use some form of intermediate storage that will need additional security. This could be, for example, the volume storage for instances running processing jobs.
	* Key management for storage may be complicated depending on the exact mechanisms used due to the distributed nature of nodes.
		* There are techniques to properly encrypt most big data storage layers today, and these align with our guidance in *Domain 11- Data Security and Encryption*. The complicating factor is that key management needs to handle distributing keys to multiple storage and analysis nodes.
	* Not all big data technologies have robust security capabilities. In some cases cloud provider security capabilities can help compensate for the big data technology limitations. Both should be included in any security architecture and the details will be specific to the combination of technologies selected.
	* Identity and Access Management will likely occur at both cloud and big data tool levels, which can complicate entitlement matrices. 
	* Many cloud providers are expanding big data support with *machine learning* and other platform as a service options that rely on access to enterprise data. These should not be used without a full understanding of potential data exposure, compliance, and privacy implications. For example, if the machine learning runs as PaaS inside the provider's infrastructure where provider employees could technically access it, does that create a compliance exposure?
		* This doesn't mean you shouldn't use the services, it means you need to understand the implications and make an appropriate risk decisions. Machine learning and other analysis services aren't necessarily insecure and don't necessarily violate privacy and compliance commitments.

### Internet of Things (IoT)

The Internet of Things is a blanket term for non-traditional computing devices used in the physical world that utilize Internet connectivity. It includes everything from Internet-enabled operational technology (used by utilities like power and water) to fitness trackers, connected lightbulbs, medical devices, and beyond. These technologies are increasingly utilized in enterprise environments for applications such as:

* Digital tracking of the supply chain.
* Digital tracking of physical logistics.
* Marketing, retail, and customer relationship management.
* Connected healthcare and lifestyle applications for employees, or delivered to consumers.

A very large percentage of these devices connect back to cloud computing infrastructure for their back end processing and data storage. Key cloud security issues related to IoT include:

* Secure data collection and sanitization. 
* Device registration, authentication, and authorization. One common issue encountered today is use of stored credentials to make direct API calls to the back-end cloud provider. There are known cases of attackers decompiling applications or device software and then using those credentials for malicious purposes.
* API security for connections from devices back to the cloud infrastructure. Aside from the stored credentials issue just mentioned, the APIs themselves could be decoded and used for attacks on the cloud infrastructure. 
* Encrypted communications. Many current devices use weak, outdated, or non-existent encryption which places data and the devices at risk.
* Ability to patch and update devices so they don't become a point of compromise. Currently, it is common for devices to be shipped as-is and never receive security updates for operating systems or applications. This has already caused multiple significant and highly publicized security incidents, such as massive botnet attacks based on compromised IoT devices.

### Mobile

Mobile computing is neither new nor exclusive to cloud, but  a very large percentage of mobile applications connect to cloud computing for their back end processing. Cloud can be an ideal platform to support mobile since cloud providers are geographically distributed and designed for the kinds of highly dynamic workloads commonly experienced with mobile applications. This section won't discuss overall mobile security but just on the portions that affect cloud security.

The primary security issues for mobile computing (in the cloud context) are very similar to IoT, except a mobile phone or tablet is also a general purpose computer:
	* Device registration, authentication, and authorization is a common source of issues. Especially (again) the use of stored credentials, and especially when the mobile device connects directly to the cloud provider's infrastructure/APIs. Attackers have been known to decompile mobile applications to reveal stored credentials which are then used to directly manipulate or attack the cloud infrastructure.
		* Data stored on the device should also be protected with the assumption that the user of the device may be a hostile attacker.
	* Application APIs are also a potential source of compromise. Attackers are known to sniff API connections, in some cases using local proxies they redirect their own devices towards, and then decompile the (likely now unencrypted) API calls and explore for security weaknesses.
		* Certificate pinning/validation inside the device application may help reduce this risk. 
		
For additional recommendations on the security of mobile and cloud computing see the latest research from the CSA [Mobile Working Group](https://cloudsecurityalliance.org/group/mobile/).

### Serverless Computing

Serverless computing is the extensive use of certain PaaS capabilities to such a degree that an all or some of an application stack runs in a cloud provider's environment without any customer-managed operating systems or even containers. "Serverless computing" is a bit of a misnomer since there is always a server running the workload someplace, but those servers and their configuration and security is completely hidden from the cloud consumer. The consumer only manages settings for the service, and not any of the underlying hardware and software stacks.

Serverless includes services such as:

	* Object storage
	* Cloud load balancers
	* Cloud databases
	* Machine learning
	* Message queues
	* Notifications services
	* Code execution environments: these are generally restricted containers where a consumer runs uploaded application code
	* API gateways
	* Web servers

Serverless capabilities may be deeply integrated by the cloud provider and tied together with event-driven systems and integrated IAM and messaging to support construction of complex applications without any customer management of servers, containers, or other infrastructure.

From a security standpoint, key issues include:

* Serverless places a much higher security burden on the cloud provider. Choosing your provider and understanding security SLAs and capabilities is absolutely critical.
* Using serverless the cloud consumer will not have access to commonly-used monitoring and logging levels, such as server or network logs. Applications will need to integrate more logging and cloud providers should provide necessary logging to meet core security and compliance requirements.
* Although the provider's services may be certified or attested for various compliance requirements, not necessarily every service will match every potential regulation. Providers need to keep compliance mappings up to date and customers need to ensure they only use services within their compliance scope.
* There will be high levels of access to the cloud provider's management plane since that is the only way to integrate and use the serverless capabilities. 
* Serverless can dramatically reduce attack surface and pathways and integrating serverless components may be an excellent way to break links in an attack chain, even if the entire application stack is not serverless.
* Any vulnerability assessment and other security testing must comply with the provider's terms of service. Cloud consumers may no longer have the ability to directly test applications, or must test with a reduced scope, since the provider's infrastructure is now hosting everything and can't distinguish between legitimate tests and attacks. 
* Incident response may also be complicated and will definitely require changes in process and tooling to manage a serverless-based incident.
	
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
	* Never transfer unencrypted keys or credentials over the Internet.
	* Test all APIs under the assumption that a hostile attacker will have authenticated, unencrypted access.
		* Consider certificate pinning and validation inside mobile applications.
		* Validate all API data and sanitize for security.
		* Implement server/cloud-side security monitoring for hostile API activity.
	* Ensure all data stored on device is secured and encrypted. 
		* Sensitive data that could allow compromise of the application stack should not be stored locally on-device where a hostile user can potentially access it.
	* Follow the more-detailed recommendations and research issued by the [CSA Mobile working group](https://cloudsecurityalliance.org/group/mobile/).
* Serverless Computing
	* Cloud providers must clearly state which PaaS services have been assessed against which compliance requirements or standards.
	* Cloud consumers must only use serverless services that match their compliance and governance obligations.
	* Consider injecting serverless components into application stacks using architectures that reduce or eliminate attack surface and/or network attack paths.
	* Understand the impacts of serverless on security assessments and monitoring.
		* Cloud consumers will need to rely more on application code scanning and logging and less on server and network logs.
	* Cloud consumers must update incident response processes for serverless deployments.
	* Although the cloud provider is responsible for security below the serverless platform level. the cloud consumer is still responsible for properly configuring and using the products.





