# Domain 11: Data Security and Encryption

## Introduction

Data security is a key enforcement tool for information and data governance. As with all areas of cloud security it should be risk-based since it is not appropriate to secure everything equally. 

This is true for data security overall, in or out of cloud, but many organizations aren't as accustomed to trusting large amounts of their sensitive data, if not all of it, with a third party, or mixing all their internal data into a shared resource pool. As such, the instinct may be to set a blanket security policy for "anything in the cloud" instead of sticking with a data and context risk-based approach, which will be far more secure and cost effective.

For example, encrypting everything in SaaS because you don't trust that provider at all likely means you shouldn't be using it in the first place. Yet encrypting everything does not necessarily solve every issue and may lead to a false sense of security, e.g. encrypting the data traffic without ensuring the security of the devices themselves.

By some perspectives information security *is* data security, but for our purposes this domain will focus on those security controls focused on securing the data itself, of which encryption is one of the most important. 

## Overview

### Data Security Controls

Data security controls tend to fall into three buckets. We cover all of these in this section:

1. Controlling what data goes into the cloud (and where).
2. Protecting and managing the data in the cloud. The key controls and processes are:
	* Access controls
	* Encryption
	* Architecture
	* Monitoring/alerting (of usage, configuration, lifecycle state, etc.)
	* Additional controls, including cloud provider-specific controls related to the specific product/service/platform of your cloud provider data loss prevention and enterprise rights management.
3. Enforcing information lifecycle management security
	* Managing data location/residency
	* Ensuring compliance, including audit artifacts (logs, configurations)
	* Backups and business continuity, which are covered in Domain 6.

### Cloud data storage types

Since cloud storage is virtualized it tends to support different data storage types than used in traditional storage technologies. Below the virtualization layer these might use well-known data storage mechanisms, but the cloud storage virtualization technologies that cloud consumers access will be different.  These are the most common:

*Object storage:* Object storage is similar to a file system. "Objects" are typically files and are then stored using a cloud platform specific mechanism. Most access is through APIs, not standard file sharing protocols, although cloud providers may also offer front-end interfaces to support those protocols.

*Volume storage:* This is essentially a virtual hard drive for instances/virtual machines. 

*Database:* Cloud platforms and providers may support a variety of different kinds of databases, including existing commercial and open source options, or their own proprietary systems. Proprietary databases typically use their own APIs. Commercial or open source databases are hosted by the provider and typically use existing standards for connections. These can be relational or non-relational, such as NoSQL and other key/value storage system or file system-based database (e.g. HDFS).

*Application/platform:* Examples of these would be a content delivery network (CDN), files stored in SaaS, caching, and other novel options.

Most cloud platforms also use redundant, durable storage mechanisms that often make use of *data dispersion* (sometimes also known as *data fragmentation of bit splitting*). This process takes chunks of data, breaks them up, and then stores multiple copies on different physical storage to provide high durability. Data stored in this way is thus physically dispersed. A single file, for example, would not be located on a single hard drive.

### Managing data migrations to the cloud

Before securing the data in the cloud most organizations want some means of managing what data is stored in private and public cloud providers. This is often essential for compliance as much, or more, than for security. 

To start, define your policies for which data types are allowed, where they are allowed, and tie these to your baseline security requirements. For example, "PII is allowed on x services assuming it meets y encryption and access control requirements".

Then identify your key data repositories. Monitor them for large migrations/activity using tools like Database Activity Monitoring and File Activity Monitoring. This is essentially building an "early warning system" for large data transfers but it’s an important data security control to detect all sorts of major breaches and misuse scenarios.

To detect actual migrations monitor cloud usage and any data transfers. You can do this with the help of the following tools:

*CASB:* Cloud Access and Security Brokers (also known as Cloud Security Gateways) discover internal use of cloud services using various mechanisms such as network monitoring, integrating with an existing network gateway or monitoring tool, or even by monitoring DNS queries. After discovering which services your users are connecting to, most of these products then offer monitoring of activity on approved services through API connections (when available) or inline interception (man in the middle monitoring). Many support DLP and other security alerting and even controls to better manage use of sensitive data in cloud services (SaaS/PaaS/and IaaS).

*URL filtering:* While not as robust as CASB a URL filter/web gateway may help you understand which cloud services your users are using (or trying to use).

*DLP:* If you monitor web traffic (and look inside SSL connections) a DLP tool may also help detect data migrations to cloud services. However, some cloud SDKs and APIs may encrypt portions of data and traffic that DLP tools can't unravel, and thus they won't be able to understand the payload.

#### Securing cloud data transfers
Ensure that you are protecting your data as it moves to the cloud. This necessitates understanding your provider’s data migration mechanisms. since leveraging provider mechanisms is often more secure and cost effective than "manual" data transfers like SFTP. For example, sending data to a provider's object storage over an API is likely much more reliable and secure than setting up your own SFTP server on a virtual machine in the same provider.

There are a few options for in-transit encryption depending on what the cloud platform supports. One way is to encrypt before sending to the cloud (clientside encryption). Network encryption (TLS/SFTP/etc.) is another option. Most cloud provider APIs use TLS by default. If not, pick a different provider since this is an essential security capability. Proxy-based encryption may a third option, where you place an encryption proxy in a trusted area between the cloud consumer and the cloud provider and the proxy manages the encryption before transferring the data to the provider.

In some instances you may have to accept public or untrusted data. If you allow partners or the public to send you data, ensure you have security mechanisms in place to sanitize it before processing or mixing with your existing data. Always isolate and scan this data before integrating it.

### Securing data in the cloud

Access controls and encryption are the core data security controls across the various technologies.:

#### Cloud data access controls

*Access controls* should be implemented with a minimum of three layers:

* *Management plane:* These are your controls for managing access of users that directly access the cloud platform's management plane. For example, logging into the web console of an IaaS services will allow that user to access data in object storage. Fortunately, most cloud platforms and providers start with default deny access control policies.

* *Public and internal sharing controls:* If data is shared externally, to the public or partners that don't have direct access to the cloud platform, there will be a second layer of controls for this access.

* *Application level controls:* As you build your own applications on the cloud platform you will design and implement your own controls to manage access.

Options for access controls will vary based on cloud service model and provider-specific features. Create an entitlement matrix based on platform specific capabilities. An entitlement matrix documents what users, groups, and roles should access which resources and functions.

Project X

|   Entitlement |   super-admin |   service-admin   |   storage-admin    |   dev |   security-audit  |   security-admin
| ----- |   ----------- |    ----------- |  ----------- |    ----------- | ----------- |
|   Volume Describe All    |   X   |   X   |       |   X   |   X   |   X   |
|   Object Describe All |   X   |       |   X   |   X   |   X   |       X   |  
|   Volume Modify  |   X   |   X   |       |   X   |       |       X   |  
|   Read Logs    |   X   |       |       |       |   X   |       X   |  
|   ... |   X   |   X   |   X   |   X   |   X   |       X   |  

 
Frequently (ideally continuously) validate your controls meet your requirements, paying particular attention to any public shares. Consider setting up alerts for all new public shares, or changes in permissions that allow public access.

*Fine Grained Access Controls and Entitlement Mappings*

The depth of potential entitlements will vary greatly from technology to technology. Some databases may support row-level security, others little more than broad access. Some will allow you to tie entitlements to identity and enforcement mechanisms built into the cloud platform, while others rely completely on the storage platform itself merely running in virtual machines.

It's important to understand your options, map them out, and build your matrix. This applies to more than file access, of course. It applies to databases and all your cloud data stores.

#### Storage (At-Rest) Encryption and Tokenization

Encryption options vary tremendously based on service model, provider, and application/deployment specifics. Key management is just as essential as the encryption option, and covered in the next section.

Encryption and tokenization are two separate technologies. Encryption protects data by applying a mathematical algorithm that "scrambles" the data, which then can only be recovered by running it through an unscrambling (decryption) process with a corresponding key. The result is a blob of ciphertext. Tokenization, on the other hand, takes the data and replaces it with a random value. It then stores the original and the randomized version in a secure database for later recovery.

Tokenization is often used when the *format* of the data is important (e.g. replacing credit card numbers in an existing system that requires the same format text string). Format Preserving Encryption encrypts data with a key but also keeps the same structural format as tokenization, but it may not be as cryptographically secure due to the compromises.

There are three components of an encryption system: Data, encryption engine, key management. The data is, of course, what you’re trying to encrypt. The engine is what performs the encryption. The key manager handles the keys for the encryption.  The overall design of the system focuses on where to put each of these components.

When designing an encryption system, you should start with a threat model. For example, do you trust a cloud provider to manage your keys? How could the keys be exposed? Where should you locate the encryption engine to manage the threats you are concerned with?

##### IaaS Encryption

IaaS volumes can be encrypted using different methods depending on your data.

*Volume storage encryption*

* *Instance-managed encryption:* The encryption engine runs within the instance, and the key is stored in the volume but protected by a passphrase or keypair. 
* *Externally managed encryption:* The encryption engine runs in the instance, but the keys are managed externally and issued to the instance on request. 

*Object and file storage*

* *Client side encryption:* When object storage is used as the back-end for an application (including mobile applications), encrypt the data using an encryption engine embedded in the application or client.
* *Server side encryption:* Data is encrypted on the server (cloud) side after being transferred in. The cloud provider has access to the key and runs the encryption engine.
* *Proxy encryption:* In this model you connect the volume to a special instance or appliance/software, and then connect your instance to the encryption instance.  The proxy handles all crypto operations and may keep keys either onboard or external.

##### PaaS Encryption

PaaS encryption varies tremendously due to all the different PaaS platforms. 

* *Application layer encryption:*  Data is encrypted in the PaaS application or the client accessing the platform. 
* *Database encryption:*  Data is encrypted in the database using encryption built in and supported by the database platform like Transparent Database Encryption (TDE) or at the field level.
* *Other:* These are provider-managed layers in the application such as the messaging queue. There are also IaaS options when that is used for underlying storage.

##### SaaS Encryption

SaaS providers may use any of the options previously discussed. It is recommended to use per-customer keys when possible to better enforce multi-tenancy isolation. The following options are for SaaS consumers:

* Provider-managed encryption.  Data is encrypted in the SaaS application and generally managed by the provider.
* Proxy encryption.  Data passes through an encryption proxy before being sent to the SaaS application.

#### Key Management (including Customer Managed Keys)

The main considerations for key management are performance, accessibility, latency, security. Can you get the right key to the right place at the right time while also meeting your security and compliance requirements?

There are four options.

* *HSM/appliance:* Use a traditional HSM or appliance-based key manager, which will typically need to be on-premise, and deliver the keys to the cloud over a dedicated connection.
* *Virtual appliance/software:* Deploy a virtual appliance or software-based key manager in the cloud.
* *Cloud provider service:* This is a key management service offered by the cloud provider. Before selecting this option, make sure you understand the security model and SLAs to understand if your key could be exposed.
* *Hybrid:* You can also use a combination, such as using a hardware HSM as the root of trust for keys but then delivering application-specific keys to a virtual appliance located in the cloud that only manages keys for its particular context.

##### Customer Managed Keys

A customer managed key allows a cloud customer to manage their own encryption key while the provider manages the encryption engine. For example, using your own key to encrypted SaaS data within the SaaS platform. Many providers encrypt data by default with keys completely in their control. Some may allow you to substitute your own key, which integrates with their encryption system. Make sure your vendor’s practices align with your requirements.

They may require you to use a service within the provider to manage the key. Thus, although the key is customer managed, it is still potentially available to provider. This doesn't necessarily mean it is insecure, since the key management and data storage systems can be separated which would require collusion on the part of multiple employees at the provider to potentially compromise data. However, keys and data could still be exposed by a government request, depending on the local laws. You may be able to store the keys external to the provider and only pass them over on a per-request basis.

### Data security architectures

Application architecture impacts data security. The features your cloud provider offers can reduce the attack surface, but make sure to demand strong metastructure security. For example, gap networks by using cloud storage or a queue service that communicates on the provider's network, not within your virtual network, thus forcing attackers to either fundamentally compromise the cloud provider or limit themselves to application-level attacks since network attack paths are closed.

An example would be using object storage for data transfers and batch processing rather than SFTP-ing to static instances. Another is message queue gapping — run application components on different virtual networks that are only bridged by passing data through the cloud provider's message queue service. This eliminates network attacks from one portion of the application to the other.

### Monitoring, auditing, and alerting

These should tie into overall cloud monitoring. (See Domains 3, 6, and 7.) Identify (and alert on) any public access or entitlement changes on sensitive data. Use tagging to support alerting, when it’s available.

You’ll need to monitor both API and storage access, since data may be exposed through either. In other words, both accessing data in object storage via an API call or through a public sharing URL. Activity monitoring, including Database Activity Monitoring, may be an option. Make sure to store your logs in secure location, like a dedicated logging account.

### Additional data security controls

#### Cloud platform/provider-specific controls

A cloud platform or provider may have data security controls not covered elsewhere in this domain. Although typically they will be some form of access control and encryption, this Guidance can't cover all possible options.

#### Data Loss Prevention

DLP is typically a way to monitor and protect data your employees access via monitoring local systems, web, email, and other traffic. It is not typically used within data centers, and thus it is more applicable to SaaS than PaaS or IaaS where it is typically not deployed.

* *CASB (Cloud Access and Security Brokers):* Some CASBs include basic DLP features for the sanctioned services they protect. For example, you could set a policy that a credit card number is never stored in a particular cloud service. The effectiveness depends greatly on the particular tool, the cloud service, and how the CASB is integrated for monitoring. Some CASB tools can also route traffic to dedicated DLP platforms for more-robust analysis than is typically available when the CASB offers DLP as a feature. 
* *Cloud provider feature:* The cloud provider themselves may offer DLP capabilities, such as a cloud file storage and collaboration platform scanning uploaded files for content and applying corresponding security policies.

#### Enterprise Rights Management

As with DLP, this is typically an employee security control that isn't always as applicable in cloud. Since all DRM/ERM is based on encryption, existing tools may break cloud capabilities, especially in SaaS.

* *Full DRM:* This is traditional full digital rights management using an existing tool. For example, applying rights to a file before storing it in the cloud service. As mentioned, it may break cloud provider features, such as browser preview or collaboration, unless there is some sort of integration (which is rare at the time of this writing).
* *Provider-based control:* The cloud platform may be able to enforce controls very similar to full DRM using native capabilities. For example, user/device/view versus edit — a policy that only allows certain users to view a file in a web browser, while other users can download and/or edit the content. Some platforms can even tie these policies to specific devices, not just on a user level.

#### Data masking and test data generation

These are techniques to protect data used in development and test environments, or to limit real-time access to data in applications.

* *Test data generation:* This is the creation of a database with non-sensitive test data based on a "real" database. It can use scrambling and other randomization techniques to create a data set that resembles the source in size and structure but lacks sensitive data.
* *Dynamic masking:* Dynamic masking rewrites data on the fly, typically using a proxy mechanism, to mask all or part of data delivered to a user. It is usually used to protect some sensitive data in applications, for example masking out all but the last digits of a credit card number when presenting to a user.

### Enforcing lifecycle management security

* *Managing data location/residency:* At certain times, you’ll need to disable unneeded locations. Use encryption to enforce access at the container or object level. Then, even if the data moves to an unapproved location, unless the key moves with the data it is still protected.
* *Ensuring compliance:* You don't merely need to implement controls to keep you compliant, you need to document and test those controls. These are "artifacts of compliance". This includes any audit artifacts you will have.
* *Backups and business continuity:* (see Domain 6)

## Recommendations

* Understand the specific capabilities of the cloud platform you are using.
* Don't dismiss cloud provider data security. In many cases it is more secure than building your own, at lower cost.
* Create an entitlement matrix for determining access controls. Enforcement will vary based on cloud provider capabilities.
* Consider CASB to monitor data flowing into SaaS. It may still be helpful for some PaaS and IaaS, but rely more on existing policies and data repository security for those types of large migrations.
* Use the appropriate encryption option based on the threat model for your data, business, and technical requirements.
* Consider use of provider-managed encryption and storage options. Where possible, use a customer managed key.
* Leverage architecture to improve data security. Don't necessarily rely completely on access controls and encryption.
* Ensure both API and data-level monitoring are in place, and that logs meet compliance and lifecycle policy requirements
* Standards exist to help establish good security and the proper use of encryption and key management techniques and processes. NIST and ANSI for example. NIST SP-800-57 and ANSI X9.69 and X9.73 specifically.