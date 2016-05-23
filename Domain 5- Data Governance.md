# Domain 5: Data Governance

> This domain is in the outline phase. The first draft will be written and opened for feedback after a review period.

> Note: due to restructuring all data security operations pieces have moved to Domain 11. This domain now limits itself to governance.

## Introduction

* Definition of data/information governance.
	* Ensuring use of data and information complies with organizational requirements, including regulatory, contractual, and organizational requirements and objectives.
* Impact of cloud on data governance
	* Multitenancy
	* Shared security responsibilities
		* Data custodianship vs. ownership
	* Jurisdictional boundaries

##Overview

* Definition of data/information governance.
	* Ensuring use of data and information complies with organizational requirements, including regulatory, contractual, and organizational requirements and objectives.
	* Data is different than information, but we tend to use them interchangeably.
		* Information is data with value.
		* For our purposes, we use both terms to mean the same thing since that is so common.
		* **note to reviewers: should we standardize on information governance?**
* Cloud Data Governance Domains
	* We will not cover all of data governance, but focus on where cloud affects data governance.
	* The cloud affects most data governance domains:
		* Information Classification. Frequently tied to compliance, and affects cloud destinations and handling requirements. Not everyone necessarily has a data classification program, but if you do, you need to adjust it for cloud.
		* Information Management Policies.  This ties to classification, and cloud needs to be added if you have them. Should also cover the different SPI tiers, since sending to a SaaS vendor vs. building your own IaaS app is very different.
		* Location and Jurisdictional Polices.  Very direct cloud implications. Any outside hosting must comply with location/jurisdiction requirements. Understand that internal policies can be changed for cloud, but legal requirements are hard lines.
			* See the legal domain for more information.
			* Understand that treaties and laws may conflict. You need to work with legal when handling regulated data to ensure you comply as best you can.
		* Authorizations. Minimal changes, but see the data security lifecycle to understand if cloud impacts.
		* Ownership. Org is always responsible, and can't abrogate that when moving to cloud.
		* Custodianship.  Cloud provider may become custodian. Data hosted but properly encrypted is still under custodianship of the org.
		* Privacy. Privacy is a sum of regulatory requirements, contractual obligations, and commitments to customers (e.g. public statements). Need to understand the total requirements and ensure information management and security policies align.
		* Contractual controls. Legal tool for extending governance requirements to a third party, like a cloud provider.
		* Security controls. Security controls are the tool to implement data governance. They change significantly in cloud. See the Data Security and Encryption domain.
	* The Data Security Lifecycle
		* A tool to help understand the security boundaries and controls around data. *Note: there will be very few changes to this from version 3*.
			* Not meant to be used as a rigorous tool for all types of data. It's a modeling tool to help evaluate data security at a high level and find focus points.
		* Phases
			* Create
			* Store
			* Use
			* Share
			* Archive
			* Destroy
		* Locations and Entitlements (update from Access)
		* Functions, Actors, and Controls
			* Functions
				* Read (*was access*)
				* Process
				* Store
			* Information lifecycle phases chart (* how is this different from data security lifecycle? *)
			* Controls chart
				* Possible and allowed
				* A control limits what is possible down to what should be allowed

## Recommendations
	* Determine governance requirements for information before planning transition to cloud. This includes legal/regulatory requirements, contractual obligations, and other corporate policies.
		* Corporate policies may need to be updated to allow a third party to handle data.
	* Ensure information governance extends to the cloud. This will be done through contractual and security controls.
	* When needed, use the data security lifecycle to help model data handling and controls.
