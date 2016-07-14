# Domain 2: Governance and Enterprise Risk Management

## Introduction

* Governance and risk management are incredibly large topics. This guidance will focus on how they change in cloud computing, and is not and should not be considered a primer or comprehensive exploration of those topics outside of cloud.
* For our purposes, we break the topics into:
    * Governance.
        * How an organization is run. Everything from the structures and policies, to the leadership and other mechanisms for management.
    * Enterprise risk management
        * Managing overall risk for the organization.
    * Information risk management
        * Managing the risk to information, which includes information technology
    * Information security
        * The tools and practices to mange risk to information
* Information security is a tool of information risk management, which is a tool of enterprise risk management, which is a tool of governance.
    * Legal issues and compliance are covered in Domains 3 and 4, respectively. Information risk management and data governance are covered in domain 5. Information security is essentially the rest of this guidance.
* It is not the role this Guidance to tell you how your organization should be governed or how to manage enterprise risks.
    * There is an incredibly wide range of maturity for governance and risk management across different organizations, compounded with just as wide a range of techniques and philosophies. These sections focus on the general implications of cloud computing on the topics.

## Overview

* Governance
    * An organization can never outsource responsibility for governance, even when using external cloud providers, you are still responsible for governance.
    * Cloud computing changes the *responsibilities* and mechanisms for implementing and managing governance.
        * These are defined in the contract.
        * Public cloud
            * You have reduced ability to govern operations in a public cloud.
            * You often have reduced ability to negotiate contracts, which impacts how you extend your governance model into the cloud.
                * Cloud providers, but the nature of their multitenancy model, can't necessarily adjust contracts and operations for each customer since everything runs on one set of resources, using one set of processes.
                    * Adapting for different customers increases costs, that's the trade off.
                * To use an analogy, think of a shipping service. When you use a common carrier/provider you don't get to define their operations. You put your sensitive documents in a package and entrust them to meet their obligations to deliver it safely, securely, and within the expected Service Level Agreement.
        * Private cloud
            * Governance isn't merely impacted by public cloud, private cloud may also have an effect.
            * If an organization allows a third party to own and/or manage the private cloud (which is very common), this is similar to how governance affects any outsourced provider.
        * Hybrid cloud
        	* When contemplating hybrid cloud environments, the governance strategy must consider the minimum common set of controls comprised of the Cloud Service Provider's contract and the organization's internal governance agreements. 
    * Tools of cloud governance:
        * Contracts
            * The primary tool of governance is the contract between a cloud provider and a cloud customer (this is true for public and private).
                * The contract is your only guarantee (short of breach of contract).
                * The contract defines the extension of governance into the cloud provider.
        * Supplier (cloud provider) assessment.
            * An assessment performed by the potential cloud customer using available information and allowed processes/techniques.
        * Compliance reporting
            * A providers self statement of compliance with their own policies/standards/commitments.
        * Audit of controls
            * A third party audit or assessment of controls.
                * Often available to cloud prospects and customers.
                * Some audits/assessments may only be available under NDA or to contracted customers.
            * Should be based on existing standards (of which there are many).
            * It's critical to understand the scope.
            * And consider the transitive trust required to treat a third party assessment as equivalent to the activities that you might undertake when doing your own assessment.
* Enterprise Risk Management
    * As with governance, the contract defines the roles and responsibilities for risk management between a cloud provider and a cloud customer.
    * There is a *shared responsibilities model* (which we most often discuss in reference to security).
        * Cloud provider accepts certain risks, and the cloud customer is responsible beyond that. This is especially evident as you evaluate differences between S/P/I.
        * It relies on good contracts and documentation to know where the lines are.
    * Risk tolerance is the amount of risk the leadership and stakeholders of an organization are willing to accept.
        * Risk tolerance varies based on asset.
        * Moving to the cloud doesn't change your risk tolerance, it just changes how risk is managed (maybe).
    * Cloud risk management trade offs:
        * Less physical control over assets and internal controls and processes.
        * Greater reliance on contracts, audits, and assessments.
        * Increased requirement for proactive management of relationship and adherence to contracts.
        * Reduced need (and cost) to manage risks the cloud provider accepts under the shared responsibility model.
    * Cloud risk management tools.
        * The supplier assessment sets the groundwork for the cloud risk management program.
            * Request or acquire documentation.
            * Review their security program and documentation.
            * Review any legal, regulatory,contractual, and jurisdictional requirements for both the provider and yourself (see the Domain 3: Legal for more).
            * Evaluate the overall provider, such as finances/stability, reputation, outsourcers.
        * Periodically review audits and assessments for current state.
            * Don't assume all services from a particular provider meet the same audit/assessment standards. They can vary.
            * Periodic assessments should be scheduled and automated if possible.
        * After reviewing and understanding what risks the cloud provider manages, what remains is residual risk.
            * Residual risk may often be managed by controls you implement (e.g. encryption).
                * The availability and implementation specifics of controls varies greatly across cloud providers and particular services/features.
            * Remaining risk must be accepted or avoided.

## Recommendations

* Understand how a contract affects your governance.
    * Don't assume you can negotiate contracts with a cloud provider, but this also shouldn't necessarily stop you from using that provider.
    * If a contract can't be negotiated and you perceive an unacceptable risk, consider alternate mechanisms to manage that risk (e.g. monitoring or encryption).
* Develop a process for cloud provider assessments.
    * This should include:
        * Contract review.
        * Self reported compliance review.
        * Documentation and policies.
        * Available audits and assessments.
    * Cloud provider re-assessments should occur on a scheduled basis and be automated if possible.
* Cloud providers should offer easy access to documentation and reports needed by cloud prospects for assessments.
    * For example, the CSA STAR registry.
* Align risk requirements to the specific assets involved and the risk tolerance for those assets.
* Use controls to manage residual risks.
    * Or accept or avoid the risks.


