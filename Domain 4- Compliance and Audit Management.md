# Domain 4: Compliance and Audit Management

## Introduction

Organizations face new challenges as they migrate from traditional data centers to the cloud. Delivering, measuring, and communicating compliance with a multitude of regulations across multiple jurisdictions is one of the largest challenges.  Customers and providers alike need to understand and appreciate the differences and implications on existing compliance and audit standards, processes, and practices. The distributed and virtualized nature of cloud requires significant adjustment from approaches based on definite and physical instantiations of information and processes. 

In addition to providers and customers, regulators and auditors are also adjusting to the new world of cloud computing.  Few existing regulations were written to account for virtualized environments or cloud deployments.  A cloud consumer can be challenged to show auditors that the organization is in compliance.  Understanding the interaction of cloud computing and the regulatory environment is a key component of any cloud strategy.  Cloud customers, auditors, and providers must consider and understand the following:

* Regulatory implications for using a particular cloud service or providers, giving particular attention to any cross-border or multi-jurisdictional issues when applicable.* Assignment of compliance responsibilities between the provider and customer, including indirect providers (i.e., the cloud provider of your cloud provider). This includes the concept of *compliance inheritance* where a provider may have parts of their service certified as compliant which removes this from the audit scope of the customer, but the customer is still responsible for the compliance of everything they build on top of the provider.* Provider capabilities for demonstrating compliance, including document generation, evidence production, and process compliance, in a timely manner.

Some additional cloud-specific issues to pay particular attention to include:

* The role of provider audits and certifications and how those affect customer audit (or assessment) scope.
* Understanding which features and services in a cloud provider are within the scope of which audits and assessments.
* Managing compliance and audits over time.
* Working with regulators and auditors who may lack experience with cloud computing technology.
* Working with providers who may lack audit and or regulatory compliance experience

## Overview

* One key area of risk management (and thus governance) that drives much of information security is managing compliance. 
	* Compliance involves the awareness and adherence to corporate obligations (e.g., corporate social responsibility, ethics, applicable laws, regulations, contracts, strategies and policies) by assessing the state of compliance, assessing the risks and potential costs of non-compliance against the costs to achieve compliance, and hence prioritize, fund, and initiate any corrective actions deemed necessary.
	* Audits are a key tool for proving (or not) compliance.
* Compliance
	* Information technology in the cloud is increasingly subject to a plethora of policies and regulations. 	* Compliance is a tool of governance. It is how an organization assesses, remediates, and proves it is meeting internal and external obligations.	* Regulations typically have strong implications for information technology and its governance, particularly in terms of monitoring, management, protection, and disclosure).
		* Many regulations and obligations require a certain level of security, which is why information security is deeply coupled with compliance. Security controls are an important tool to assure compliance, and evaluation and testing of these controls is a core activity for security professionals.
			* Even when the assessments of security controls are performed by dedicated internal or external auditors.
	* How Cloud Changes Compliance
		* As with security, compliance in the cloud is a shared responsibility model. Both the cloud provider and customer have responsibilities, but the customer is always ultimately responsible for their own compliance.
			* These are defined through contracts, audits/assessments, and specifics of the compliance requirements.
		* Cloud customers, particularly in public cloud, must rely more on third-party attestations of the provider to understand their compliance alignment and gaps.
			* Since public cloud providers rely on economies of scale to manage costs they often will not allow customers to perform their own audits. Instead, similar to financial audits of public companies, they engage with a third-party firm to perform audits and issue attestations.
			* This the cloud customer doesn't get to define the scope or perform the audit themselves, but they will need to rely on these reports and attestations to determine if the service meets their compliance obligations.
		* Many cloud providers are certified for various regulations and industry requirements, such as PCI DSS, SOC1, SOC2, HIPAA, best practices/frameworks like CSA CCM, and global/regional regulations like the EU GDPR. These are sometimes referred to as *pass-through audits*. 
			* A pass through audit is a form of *compliance inheritance*.
				* In this model all or some of the cloud provider's infrastructure and services undergo an audit to a compliance standard. The provider takes responsibility for the costs and maintenance of these certifications.
				* They certify that the *provider* is compliant.
				* It is still the responsibility of the customer to *build compliant applications and services on the cloud*.
				* This means the provider's infrastructure/services are not within scope of a customer's audit/assessment. But everything the customer builds themselves is still within scope.
				* The customer is still ultimately responsible for maintaining the compliance of what they build and manage.
				* For example, if an IaaS provider is PCI DSS certified, the customer can build their own PCI-compliant service on that platform and the provider's infrastructure and operations should be outside the *customer's* assessment scope. However, the customer can just as easily run afoul of PCI and fail their assessment if they don't design their own application running in the cloud properly.
		* Many cloud providers offer globally distributed data centers running off a central management console/platform. It is still the customer's responsibility to manage and understand where to deploy data and services and still maintain their legal compliance across national and international jurisdictions.
			* Organizations have the same responsibility in traditional computing, but the cloud dramatically reduces the friction of these potentially-international deployments. E.g. a developer can potentially deploy regulated data in a non-compliant country without having to request an international data center and sign off on multiple levels of contracts, should the proper controls not be enabled to prevent this.
		* Not all features and services within a given cloud provider are necessarily compliant and certified/audited with respect to all regulations and standarxs. It is incumbent on the cloud provider to communicate certifications and attestations clearly, and for customers to understand the scopes and limitations.
* Audit Management
	* Proper organizational governance naturally includes audit and assurance.  Audit must be independently conducted and should be robustly designed to reflect best practice, appropriate resources, and tested protocols and standards.
	* Audits and assessments are mechanisms to *document compliance* with internal or external requirements (or identify deficiencies).
	* Audit and assessment reporting needs to include a compliance determination as well a list of identified issues, risks and remediation recommendations.
	* Audits and assessments related to information security typically focus on evaluating the effectiveness of security management and controls.
	* Most organizations are subject to a mix of internal and external audits and assessments to assure compliance with internal and external requirements. 
	* Audits have variable scope and statement of applicability, which defines what is evaluated (e.g. all systems with financial data) and to which controls (e.g. an industry standard, custom scope, or both).
		* An attestation is a legal statement from a third party, which can be used as their statement of audit findings.
	* Audit management includes the management of all activities related to audits and assessments, such as determining requirements, scope, scheduling, and responsibilities.
	* How cloud changes audit management:
		* Some cloud customers may be used to auditing third party providers, but the nature of cloud computing and contracts with cloud providers will often preclude things like on-premise audits. 
			* Customers should understand that providers can (and often should) consider on-premise audits a security risk when providing multitenant services. Multiple on-premise audits from large numbers of customers presents clear logistical and security challenges, especially when the provider relies on shared assets to create the resource pools.
				* Customers will rely more on third party attestations rather than audits they perform themselves.
				* Depending on the audit standard, actual results may only be releasable under an NDA. This is often due to legal or contractual requirements with the audit firm, not due to any attempts and obfuscation by the cloud provider.
			* Cloud providers should understand that customers still need assurance that the provider meets their contractual and regulatory obligations, and should thus provide rigorous third party attestations to prove they meet their obligations. 
				* These should be based on industry standards, with clearly defined scopes and the list of specific controls evaluated.
				* Publishing certifications and attestations (to the degree legally allowed) will greatly assist cloud customers in evaluating providers.
		* Some standards, like SSAE 16, attest that documented controls work as designed/required. The standard doesn't necessarily define the *scope of controls*, so both are needed to perform a full evaluation.
		* Attestations and certifications don't necessarily apply equally to all services offered by a cloud provider. 
			* Providers should be clear about which services and features are covered, and it is the responsibility of the customer to pay attention and understand the implications on their use of the provider.
		* Certain types of assessments and audits (such as a vulnerability assessment) may be limited in the provider's terms of service, and may require permission. This is often to help the provider distinguish between a legitimate assessment and an attack.
		* Attestations and certifications are point-in-time activities.
			* An attestation is a statement of an "over a period of time" assessment.
			* Providers must keep any published results current.
			* Customers are responsible for ensuring they rely on current results.
		* *Artifacts* are the logs, documentation, and other materials needed for audits and compliance.
			* They are the evidence to support compliance activities.
			* Both providers and customers have responsibilities for producing and managing their respective artifacts.
			* Customers are ultimately responsible for the artifacts to support their own audits, and thus need to know what the provider offers, and create their own artifacts to cover any gaps. For example, by building in more robust logging into an application since server logs on PaaS may not be available.

## Recommendations

* Compliance, audit, and assurance should be *continuous*. They should not be seen as merely point in time activities, and many standards and regulations are moving more towards this model. This is especially true in cloud computing where both the provider and customer tend to be in more-constant flux and are rarely ever in a static state.
* Cloud providers should:
	* Clearly communicate their audit results, certifications, and attestations with particular attention to:
		* The scope of assessments.
		* Which specific features/services are covered in which locations and jurisdictions.
		* How customer can deploy compliant applications and services on the cloud.
		* Any additional customer responsibilities and limitations.
	* Cloud providers must maintain their certifications/attestations over time and proactively communicate any changes in status.
	* Cloud providers should engage in continuous compliance initiatives to avoid creating any gaps, and thus exposures, for their customers.
	* Provide customers commonly needed evidences and artifacts of compliance, such as logs of administrative activity the customer cannot otherwise collect on their own.
* Cloud customers should:
	* Understand their full compliance obligations before deploying or migrating to, or developing in, the cloud.
	* Evaluate a provider's third-party attestations and certifications and align those to compliance needs.
	* Understand the scope of assessments and certifications, including both the controls and the features/services covered.
	* Attempt to select auditors with experience in cloud computing, especially if pass-through audits and certifications will be used to manage the customer's audit scope.
	* Ensure they understand what artifacts of compliance the provider offers, and effectively collect and manage those artifacts.
		* Create and collect their own artifacts when the provider's artifacts are not sufficient.



