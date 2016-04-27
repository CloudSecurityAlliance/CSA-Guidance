# Domain 11: Data Security and Encryption

## Introduction

* Data security is the enforcement of data governance.
	* Must take a risk-based approach, not appropriate to secure everything equally. 
	* Must account for the cloud provider's security controls and *trust*. 
		* Cloud security is a shared responsibility. You lose the economic benefits if you don't understand or trust the cloud provider. The focus is on implementing controls that are either outside the cloud provider's domain, or *when, after a risk assessment, you need additional security to manage a provider risk*.
		* For example, encrypting everything in SaaS because you don't trust that provider at all likely means you shouldn't be using it in the first place.
	* Data security for cloud computing key domains:
		* Controlling data migrations to the cloud.
		* Protecting data in the cloud
			* Encryption is often a key control.
		* Monitoring and auditing activity
		* Enforcing lifecycle governance

## Overview 

* Data security controls tend to fall into three buckets:
	* Controlling what data goes into the cloud (and where).
	* Protecting and managing the data in the cloud.
		* Access controls
		* Encryption
		* Architecture
		* Monitoring/alerting
		* Additional controls
			* Cloud provider-specific controls
			* Data Loss Prevention
			* Enterprise rights Management
	* Enforcing lifecycle management security
		* Managing data location/residency
		* Ensuring compliance
			* Including audit artifacts
		* Backups and business continuity (see Domain 6)
* Cloud data storage types
	* Use of data dispersion/fragmentation
	* Object
		* File system
	* Volume
	* Database
		* Relational
		* Non-relational
			* File system (e.g. HDFS)
	* Application/platform
		* E.g. a CDN, files stored in SaaS, caching, and other novel options.
* Managing data migrations to the cloud
	* Define policies for allowed data types
		* Tie to baseline security requirements. E.g. "PII is allowed on x services assuming it meets y encryption and access control requirements".
	* Identify key data repositories
		* Monitor for large migrations/activity
	* Monitor cloud usage and data migrations
		* CASB
		* URL filtering
		* DLP
	* Protecting data as it moves to the cloud
		* Understand provider data migration mechanisms
			* Leveraging provider mechanisms is often more secure and cost effective than "manual" data transfers like SFTP.
		* In-transit encryption options
			* Encrypt before sending to the cloud (clientside encryption)
			* Network encryption (TLS/SFTP/etc.)
			* Proxy-based encryption
		* Accepting public/untrusted data
			* Isolate and scan
	* Access controls
		* Minimum of three layers
			* Management plane
			* Public and internal sharing controls
			* Application level controls
		* Options will vary based on cloud service model and provider-specific features
		* Create an entitlement matrix based on platform specific capabilities
		* Frequently (ideally continuously) validate controls meet requirements
			* Pay particular attention to any public shares
			* Consider alerts for all new public shares/changes in permissions that allow public access
		* Fine Grained Access Controls and Entitlement Mappings
			* Applies to more than file access... e.g. database.
	* Storage (At-Rest) Encryption and Tokenization
			* Options vary tremendously based on service model, provider, and application/deployment specifics.
				* Key management just as essential as the encryption option, and covered in the next section.
			* Encryption vs. tokenization
				* Format Preserving Encryption
			* The three components of an encryption system
				* Data, encryption engine, key management
				* Threat model
			* IaaS
				* Volume storage encryption
					* Instance-managed
					* Externally managed
				* Object and file storage
					* Client Side Encryption
					* Server Side Encryption
					* Proxy Encryption
			* PaaS
				* Varies tremendously due to all the different PaaS platforms
				* Application layer (encrypt in the application before storing)
				* Database
					* Transparent Database Encryption (TDE)
					* Field level
				* Other (e.g. messaging queue)
					* Provider-managed
					* Application layer
				* IaaS options when that is used for underlying storage
			* SaaS
				* Provider managed
				* Proxy
		* Key Management for Cloud Computing
			* Considerations
				* Performance, accessibility, latency, security
			* Options
				* HSM/appliance
				* Virtual appliance/software
				* Cloud provider service
				* Hybrid
			* Customer-managed keys for cloud provider services
				* Provider vs. customer managed
				* Internal vs. external service
* Data security architectures
		* Application architecture impacts data security
		* Cloud provider features can reduce attack surface, but also demand strong metastructure security
			* E.g. gap networks by using cloud storage or queue service
		* Some examples
			* Object storage for data transfers and batch processing vs. SFTP to static instances
			* Message queue gapping
* Monitoring, auditing, and alerting
		* Should tie into overall cloud monitoring
			* See Domains 3, 6, and 7.
		* Identify (and alert on) public access or entitlement changes on sensitive data
			* Use tagging to support alerting, when available
		* Monitor both API and storage access, since data may be exposed through either
			* e.g. access object storage via an API call vs. a public sharing URL.
		* Activity monitoring, including Database Activity Monitoring, may be an option.
		* Store logs in secure location (e.g. a dedicated logging account)
* Additional controls
	* Cloud provider-specific controls
	* Data Loss Prevention
		* CASB
			* Dedicated DLP integration
		* Cloud provider feature
		* Dedicated DLP typically not deployed in IaaS/PaaS.
	* Enterprise Rights Management
		* Full DRM
		* Provider-based control
			* E.g. user/device/view vs. edit
	* Data masking and test data generation
		* Test data generation
		* Dynamic masking
* Enforcing lifecycle management security
	* Managing data location/residency
		* Disable unneeded locations
		* Encryption to enforce
	* Ensuring compliance
		* Including audit artifacts
	* Backups and business continuity (see Domain 6)

## Recommendations

* Understand the specific capabilities of the cloud platform you are using.
* Don't dismiss cloud provider data security. In many cases it is more secure than building your own, at lower cost.
* Create an entitlement matrix for determining access controls.
	* Enforcement will vary based on cloud provider capabilities.
* Consider CASB to monitor data flowing into SaaS
	* May still be helpful for some PaaS and IaaS, but rely more on existing policies and data repository security for those types of large migrations.
* Use the appropriate encryption option based on the threat model for your data, business, and technical requirements.
* Consider use of provider-managed encryption and storage options.
	* Where possible, use a customer managed key.
* Leverage architecture to improve data security. Don't necessarily rely completely on access controls and encryption.
* Ensure both API and data-level monitoring are in place.
	* And that logs meet compliance and lifecycle policy requirements