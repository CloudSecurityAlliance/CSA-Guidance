# Domain 2: Governance and Enterprise Risk Management

## Introduction

Governance and risk management are incredibly large topics. This guidance focuses on how they change in cloud computing and is not and should not be considered a primer or comprehensive exploration of those topics outside of cloud.

For security professionals, cloud computing impacts four areas of governance and risk management:
    
* Governance includes the policy, process and internal controls that comprise how an organization is run. Everything from the structures and policies, to the leadership and other mechanisms for management.

> For more information on governance please see 
	* ISO/IEC 38500:2015 - Information Technology - Governance of IT for the organization (http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=62816)
	* ISACA - COBIT - A Business Framework for the Governance and Management of Enterprise IT  (http://www.isaca.org/cobit/Pages/CobitFramework.aspx)
	* ISO/IEC 27014:2013 - Information Technology - Security techniques - Governance of information security (http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=43754)
    
* Enterprise risk management includes managing overall risk for the organization, aligned to the organization's governance and risk tolerance. Enterprise risk management includes all areas of risk, not merely those concerned with technology.

* Information risk management covers managing the risk to information, including information technology. Organizations face all sorts of risks, from financial to physical, and information is only one of multiple assets an organization needs to manage. 

* Information security is the tools and practices to mange risk to information. Information security isn't the be all and end all of managing information risks; policies, contracts, insurance, and other mechanisms also play a role (including physical security for non-digital information). However, a, if not the, primary role of information security is to provide the processes and controls to protect electronic information and the systems we use to access it.

Information security is a tool of information risk management, which is a tool of enterprise risk management, which is a tool of governance. The four are all closely related but require individual focus, processes, and tools. 
    
> Legal issues and compliance are covered in Domains 3 and 4, respectively. Information risk management and data governance are covered in domain 5. Information security is essentially the rest of this guidance.

## Overview

### Governance
Cloud computing affects governance since it either introduces a third party into the process (with public cloud or hosted private cloud) or potentially alters internal governance structures with self-hosted private cloud. The primary issue to remember when governing cloud computing is that *an organization can never outsource responsibility for governance*, even when using external providers. Since cloud service providers try to leverage economies of scale to manage costs and enable capabilities governance can't necessarily treat them the same as dedicated external service providers that customize their offerings more for each client.

Cloud computing changes the *responsibilities* and mechanisms for implementing and managing governance. Responsibilities and mechanisms for governance are defined in the contract. If the area of concern isn't in the contract, there are no mechanisms available to enforce, and there is a governance gap. Governance gaps don't necessarily exclude using the provider, but they do require the customer to adjust their own processes to close the gaps or accept the associated risks.

#### Tools of cloud governance

These are the specific management tools used for managing governance. This list focuses more on tools for external providers but these same tools can often be used internally for private deployments:

* Contracts

The primary tool of governance is the contract between a cloud provider and a cloud customer (this is true for public and private). The contract is your only guarantee, assuming there is no breach of contract which tosses everything into a legal scenario. Contracts are the primary tool to extend governance into business partners and providers.

* Supplier (cloud provider) Assessments

These assessments are performed by the potential cloud customer using available information and allowed processes/techniques. They combine contractual and manual research with third party attestations and technical research. They are very similar to any supplier assessment, and can include aspects like financial viability, history, feature offerings, third party attestations, feedback from peers, and so on. More detail on assessments is covered later in this Domain and in Domain 4.

* Compliance reporting

Compliance reporting includes all the documentation on a provider's internal (self) and external compliance assessments. They are the reports from *audits of controls*, which an organization can perform themselves, a customer can perform on a provider (although this usually isn't an option in cloud), or have performed by a trusted third-party. Third-party audits and assessments are preferred since they provide independent validation (assuming you trust the third-party).
        
Compliance reports are often available to cloud prospects and customers but may only be available under NDA or to contracted customers. This is often required by the firm that performed the audit and isn't necessarily something completely under the control of the cloud provider.

Assessments and audits should be based on existing standards (of which there are many). It's critical to understand the scope, not just the standard used. Standards like the SSAE 16 have a defined scope, which includes both *what* is assessed (e.g. which of the provider's services) and *which controls* are assessed. A provider can thus "pass" an audit that doesn't include any security controls, which isn't overly useful for security and risk managers. Also consider the transitive trust required to treat a third party assessment as equivalent to the activities that you might undertake when doing your own assessment. Not all audit firms (or auditors) are created equal. This should be included in your governance decisions.

### Enterprise Risk Management
Enterprise Risk Management (ERM) is the overall management of risk for an organization. As with governance, the contract is the central definition of the roles and responsibilities for risk management between a cloud provider and a cloud customer. And, as with governance, you can never outsource your overall responsibility and accountability for risk management to an external provider.

> For more on risk management see 
	* ISO 31000:2009 - Risk management – Principles and guidelines (http://www.iso.org/iso/catalogue_detail?csnumber=43170)
	* ISO/IEC 31010:2009 - Risk management – Risk assessment techniques (http://www.iso.org/iso/catalogue_detail?csnumber=51073)
	* NIST Special Publication 800-37 Revision 1 (updated June 5, 2014) (http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-37r1.pdf)
    
Risk management in cloud is based on the *shared responsibilities model* (which we most often discuss in reference to security). The cloud provider accepts some responsibility for certain risks, and the cloud customer is responsible beyond that. This is especially evident as you evaluate differences between the service models, where the provider manages more risks in SaaS and the consumer more in IaaS. But, again, the cloud consumer is ultimately responsible for ownership of the risks, they only pass on some of the *risk management* to the cloud provider. These hold true even with a self-hosted private cloud where in those situations an organizational unit is passing on some of their risk management to the internal cloud provider instead of an external party, and internal SLAs and procedures replace external contracts.

ERM relies on good contracts and documentation to know where the division of responsibilities and potential for untreated risk lie. Where governance is nearly exclusively focused on contracts, risk management can delve deeper into the technology and process capabilities of the provider, based on their documentation. For example, a contract will rarely define how network security is actually implemented. Review of the provider's documentation will provide much more information to help with an effective risk decision.

*Risk tolerance* is the amount of risk the leadership and stakeholders of an organization are willing to accept. It varies based on asset and you shouldn't make a blanket risk decision about a particular provider but rather assessments should align with the value and requirements of the assets involved. Just because a public cloud provider is external and a consumer might be concerned with shared infrastructure for some assets doesn't mean it isn't within risk tolerance for all assets. Over time this means practically speaking, that you will build out a matrix of cloud services and which types of assets are allowed in those services. Moving to the cloud doesn't change your risk tolerance, it just changes how risk is managed.

### The Effects of Service Model and Deployment Model

In considering the various options available not only in Cloud Service Providers but also in the fundamental delivery of cloud services, attention must be paid to how the Service and Deployment models effect the ability to manage governance and risk.  

#### Service Models

##### Software as a Service (SaaS)

In the majority of cases, SaaS presents the most critical example of the need for a negotiated contract to protect the ability to govern or validate risk as it relates to data stored, processed, and transmitted with and in the application. SaaS providers tend to cluster at either end of the size/capability spectrum and the likelihood of a negotiated contract is much higher when dealing with a small SaaS provider. Unfortunately, many small SaaS providers are not able to operate at a level of sophistication that matches or exceeds customer governance and risk management capabilities. In concrete terms, the entire level of visibility into the actual operation of the infrastructure that provides the SaaS application is limited to solely the user interface developed by the Cloud Provider. For many governance and risk management activities, the information necessary to drive decisions is simply unavailable in the sense that it is all subject to filtering through that application interface. 

##### Platform as a Service (PaaS)

Continuing through the Service Models, the level of detail that is available (and the consequential ability to self-manage governance and risk issues) increases. The likelihood of a fully negotiated contract is likely lower than with either of the other service models as the core driver for most PaaS is to deliver a single capability with very high efficiency. PaaS is typically delivered with a rich API, many PaaS providers have enabled the collection of some data necessary to prove SLAs are being adhered to - but the customer is still in the position of having to exercise a significant effort in determining if contract stipulations are effectively providing the level of control or support that is required to enable governance or risk management. 

##### Infrastructure as a Service (IaaS)

Infrastructure as a Service represents the closest that Cloud comes to a traditional data center (or even a traditional outsourced managed data center) and the vast majority of the existing governance and risk management activities that organizations have already built and utilize are directly transferable. There are however, new complexities related to the underlying orchestration and management layers that enable the infrastructure which are often overlooked. In many ways, the governance and risk management of that orchestration and management layer is consistent with the underlying infrastructure (network, power, HVAC, etc.) of a traditional data center and the same governance and risk management issues are there but the exposure of those systems is sufficiently different that a change to the existing process is required. 

#### Deployment Models
	 
##### Public cloud

Cloud customers have a reduced ability to govern operations in a public cloud since the provider is responsible for the management and governance of their infrastructure, employees, and everything else. The customers also often have reduced ability to negotiate contracts, which impacts how they extend their governance model into the cloud. Inflexible contracts are a natural property of multitenancy. Providers can't necessarily adjust contracts and operations for each customer as everything runs on one set of resources, using one set of processes. Adapting for different customers increases costs, causing a trade off and often the dividing line between using public and private cloud. Hosted private cloud allows full customization, but at increased costs due to the loss of the economies of scale.

This doesn't mean you shouldn't try to negotiate your contract, but recognize this isn't always possible and you instead will need to either choose a different provider (who may actually be less secure), or adjust your needs and use alternate governance mechanisms to mitigate concerns.

To use an analogy, think of a shipping service. When you use a common carrier/provider you don't get to define their operations. You put your sensitive documents in a package and entrust them to meet their obligations to deliver it safely, securely, and within the expected Service Level Agreement.

##### Private cloud

Governance isn't merely impacted by public cloud, private cloud will also have an effect. If an organization allows a third party to own and/or manage the private cloud (which is very common), this is similar to how governance affects any outsourced provider. There will be shared responsibilities with obligations defined in the contract. 

Although you will likely have more control over contractual terms it's still important to ensure they provide you with the needed governance mechanisms. As opposed to a public provider who has various incentives to keep their service well-documented and at particular standard levels of performance, functionality, and competitiveness a hosted private cloud may only offer exactly what is in the contract with everything else at extra cost. This *must* be considered and accounted for in negotiations with clauses to guarantee the platform itself remains up to date and competitive.

With a self-hosted private cloud governance will focus on internal service level agreements for the cloud consumers (business or other organizational units) and chargeback and billing models for providing access to the cloud.

##### Hybrid and Community Clouds

When contemplating hybrid cloud environments, the governance strategy must consider the minimum common set of controls comprised of the Cloud Service Provider's contract and the organization's internal governance agreements. The cloud consumer is connecting either two cloud environments, or a cloud environment and a data center. In either case the overall governance is the intersection of those two models. For example, if you connect your data center to your cloud over a dedicated network link you need to account for governance issues that will span both environments.

Since community clouds are a shared platform with multiple organizations, but not public, governance extends to the relationships with those members of the community, not just the provider and the customer. It is a mix of how you would approach public cloud and a hosted private cloud governance, where the overall tools of governance and contracts will have some of the economies of scale of a public cloud provider, but be tunable based on community consensus more like a hosted private cloud.


#### Cloud risk management trade offs:
There are advantages and disadvantages to managing enterprise risk for cloud deployments. These factors are, as you would expect, more pronounced for public cloud and hosted private cloud:

* There is less physical control over assets and their controls and processes. You don't physically control the infrastructure or the provider's internal processes.
* There is a greater reliance on contracts, audits, and assessments as you lack day to day visibility or management.
* This creates an increased requirement for proactive management of relationship and adherence to contracts that extends beyond the initial contract signing and audits. Cloud providers also constantly evolve their products and services to remain competitive and these ongoing innovations might exceed, strain, or not be covered by existing agreements and assessments. 
* Cloud customers have a reduced need (and associated reduction in costs) to manage risks the cloud provider accepts under the shared responsibility model. You haven't outsourced accountability for managing the risk, but you can certainly outsource the management of the risk.

#### Cloud risk management tools

These processes help form the foundation of managing risk in cloud computing deployments. One of the core tenants of risk management is you can *manage, transfer, accept, or avoid* risks. But everything starts with a proper assessment:

The supplier assessment sets the groundwork for the cloud risk management program:
	* Request or acquire documentation.
	* Review their security program and documentation.
	* Review any legal, regulatory, contractual, and jurisdictional requirements for both the provider and yourself (see the Domain 3: Legal for more).
	* Evaluate the contracted service in the context of your information assets.
	* Separately evaluate the overall provider, such as finances/stability, reputation, outsourcers.

Periodically review audits and assessments to ensure they are up to date:
	* Don't assume all services from a particular provider meet the same audit/assessment standards. They can vary.
	* Periodic assessments should be scheduled and *automated* if possible.

After reviewing and understanding what risks the cloud provider manages, what remains is *residual risk*. Residual risk may often be managed by controls you implement (e.g. encryption). The availability and implementation specifics of risk controls varies greatly across cloud providers, particular services/features, service models, and deployment models. If, after all your assessments and the controls you implement yourself there is still residual risk your only options are to transfer it, accept the risk, or avoid it.

Risk transfer, most often enabled by insurance, is an imperfect mechanism, especially for information risks. It can compensate some of the financial loss associated with a primary loss event but won't help with a secondary loss event especially an intangible or difficult to quantify loss such as reputation. From the perspective of insurance carriers, cyber-insurance is also a nascent field without the depth of actuarial tables used for other forms of insurance, like those of fire or flooding, and even the financial compensation may not match the costs associated with the primary loss event. Understand the limits.

## Recommendations

* Identify the shared responsibilities of security and risk management based on the chosen cloud deployment *and* service model. Develop a Cloud Governance Framework/Model as per relevant industry best practices, global standards and regulations like CSA CCM, COBIT 5, NIST RMF, ISO/IEC 27017, HIPAA, PCI DSS, EU GDPR etc.
* Understand how a contract affects your governance framework/model.
	* Obtain and review contracts (and any referenced documents) before entering into an agreement.
    * Don't assume you can effectively negotiate contracts with a cloud provider, but this also shouldn't necessarily stop you from using that provider.
    * If a contract can't be effectively negotiated and you perceive an unacceptable risk, consider alternate mechanisms to manage that risk (e.g. monitoring or encryption).
* Develop a process for cloud provider assessments.
    * This should include:
        * Contract review.
        * Self reported compliance review.
        * Documentation and policies.
        * Available audits and assessments.
        * Service reviews adapting to the customer’s requirements
		* Strong Change management policies
    * Cloud provider re-assessments should occur on a scheduled basis and be automated if possible.
* Cloud providers should offer easy access to documentation and reports needed by cloud prospects for assessments.
    * For example, the CSA STAR registry.
* Align risk requirements to the specific assets involved and the risk tolerance for those assets.
* Create a specific risk management and risk acceptance/mitigation methodology to assess the risks of every solution in the space
* Use controls to manage residual risks.
    * If residual risks remain, choose to accept or avoid the risks.
* Use tooling to track approved providers based on asset type (e.g. linked to data classification), cloud usage, and management.


