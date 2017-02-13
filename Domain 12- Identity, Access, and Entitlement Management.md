# Domain 12: Identity, Entitlement, and Access Management 

## Introduction

Identity, entitlement, and access management (IAM) are deeply impacted by cloud computing. In both public and private cloud they require two parties to manage IAM without compromising security. This domain focuses on what needs to change in identity management for cloud. While we review some fundamental concepts, the focus is on how cloud changes identity management, and what to do about it. 

Cloud computing introduces multiple changes to how we have traditionally managed IAM for internal systems. It isn't that these are necessarily new issues, it is that they are bigger issues. 

The key difference is the relationship between the cloud provider and the cloud consumer, even in private cloud. IAM can't be managed solely by one or the other and thus a trust relationship, designation of responsibilities, and the technical mechanics to enable them are required. More often than not this comes down to *federation*. This is exacerbated by the fact that most organizations have many (sometimes hundreds) of different cloud providers they need to extend their IAM into.

Cloud also tends to change faster, be more distributed (including across legal jurisdictional boundaries), adds the complexity of the management plane, and relies more (exclusively) on broad network communications for everything (opening up core infrastructure administration to network attacks). Plus there are extensive differences between providers and between the different service and deployment models.

> This domain focuses primarily on IAM between an organization and cloud providers or between cloud providers and services. It does not discuss all the aspects of managing IAM within a cloud application, such as the internal IAM for an enterprise application running on IaaS since those issues are very similar to building similar applications and services in traditional infrastructure. 

### How IAM is different in the cloud:

Identity and access management is always complicated. At the heart we are mapping some form of an entity (a person, system, piece of code, etc.) to a verifiable identity associated with various attributes (which can change based on current circumstances), and then making a decision on what they can or can't do based on entitlements. Even when you control the entire chain of that process, managing it across disparate systems and technologies, in a secure and verifiable manner, especially at scale, is a challenge,

In cloud computing, the fundamental problem is that multiple organizations are now managing the identity and access management to resources which can greatly complicate the process. For example, imagine having to provision the same user on dozens, or hundreds, of different cloud services. *Federation* is the primary tool to manage this problem by building trust relationships between organizations, and enforcing them through standards-based technologies.

Federation and other IAM techniques and technologies have existed since before the first computers (just ask a bank or government), and over time many organizations have built patchworks and silos of IAM as their IT has evolved. Cloud computing is a bit of a forcing function since adopting cloud very quickly pushes organizations to confront their IAM practices and update them to deal with the differences of cloud. This brings both opportunities and challenges.

At a high level, the migration to cloud is an opportunity to build new infrastructure and processes on modern architectures and standards. There have been tremendous advances in IAM over the years, yet many organizations have only been able to implement them in limited use cases due to budget and legacy infrastructure constraints. The adoption of cloud computing, be it a small project or an entire data center migration, means building new systems on new infrastructure that are generally architected using the latest IAM practices.

These shifts also bring challenges. Moving to federation at scale with multiple internal and external parties can be complex and difficult to manage due to the sheer mathematics of all the variables involved. Determining, and enforcing, attributes and entitlements across disparate systems and technologies bring both process and technical issues. Even fundamental architectural decisions may be hampered by the wide variation in support among cloud providers and platforms. 

IAM spans essentially every domain in this document. This section starts with a quick review of some core terminology that not all readers may be familiar with, then delves into the cloud impacts on first identity, then access and entitlement management. 
 
 ## Overview
 
IAM is a broad are of practice with its own lexicon of terminology that can be confusing for those who aren't domain specialists, especially since some have different meanings in different contexts (and they are used in areas outside IAM). Even the term "IAM" is not universal and is often referred to as *Identity Management (IdM)*.

Gartner defines IAM as ["the security discipline that enables the right individuals to access the right resources at the right times for the right reasons."](http://www.gartner.com/it-glossary/identity-and-access-management-iam/) Before we get into the details here are the high level terms most relevant to our discussion of IAM in cloud computing:

* *Entity:* the person or "thing" that will have an identity. It could be an individual, a system, a device, or application code. 
* *Identity:* the unique expression of an entity within a given namespace. An entity can have multiple digital identities, such as a single individual having a work identity (or even multiple identities, depending on the systems), a social media identity, and a personal identity. For example, if you are a single entry in a single directory server that is your identity.
* *Identifier:* the means by which an identity can be asserted. For digital identities this is often a cryptological token. In the real world it might be your passport.
* *Attributes:* facets of an identity. Attributes can be relatively static (like an organizational unit) or highly dynamic (IP address, device being used, if the user authenticated with MFA, location, etc.). 
* *Persona:* the expression of an identity with attributes that indicates context. For example, a developer that logs into work and then connects to a cloud environment as a developer on a particular project. The identity is still the individual, and the persona is the individual in the context of that project.
* *Role:* identities can have multiple roles which indicate context. "Role" is a confusing and abused term used in many different ways. For our purposes we will think of it as similar to a persona, or a subset of a persona. For example, a given developer on a given project may have different roles which are then used to make access decisions, such as "super-admin" and "dev".
* *Authentication:* the process of confirming an identity. When you log in to a system you present a username (the identifier) and password (an attribute we refer to as an authentication factor). Also known as *Authn*.
* *Multifactor Authentication (MFA):* use of multiple factors in authentication. Common options include one time passwords generated by a physical or virtual device/token (OTP), out of band validation through an OTP sent via text message or confirmation from a mobile device. biometrics, or plug-in tokens.
* *Access control:* restricting access to a resource. *Access management* is the process of managing access to the resources.
* *Authorization:* allowing an identity access to something (e.g. data or a function). Also known as *Authz*.
* *Entitlement:* mapping an identity (including roles, personas, and attributes) to an authorization. The entitlement is what they are allowed to do, and for documentation purposes we keep these in an *entitlement matrix*.
* *Federated Identity Management:* the process of asserting an identity across different systems or organizations. This is the key enabler of *Single Sign On* and also core to managing IAM in cloud computing.
* *Authoritative source:* the "root" source of an identity, such as the directory server that manages employee identities. 
* *Identity Provider:* the source of the identity in federation. The identity provider isn't always the authoritative source, but can sometimes rely on the authoritative source, especially if it is a broker for the process.
* *Relying Party:* the system that relies on an identity assertion from an identity provider.

> Create a diagram showing entity > identities (with identifiers and attributes) > Persona > Role

There are a few more terms that will be covered in their relevant sections below, including the major IAM standards. Also, although this domain may seem overly focused on public cloud all the same principles apply in private cloud, although the scope will be less since the organization may have more control over the entire stack.

### IAM standards for cloud computing

There are quite a few identity and access management standards out there and many of them can be used in cloud computing. Despite the wide range of options the industry is settling on a core set that are most commonly seen in various deployments and are supported by the most providers. There are also some standards that are promising but aren't yet as widely used. This list doesn't reflect any particular endorsement and doesn't include all options, but is merely representative of what is most commonly supported by the widest range of providers:

* *Security Assertion Markup Language (SAML):* SAML 2.0 is an OASIS standard for federated identity management that supports both authentication and authorization. It uses XML to make assertions between an identity provider and a relying party, Assertions can contain authentication statements, attribute statements, and authorization decision statements. SAML is very widely supported by both enterprise tools and cloud providers but can be complex to initially configure.
* *OAuth:* is an IETF standard for authorization that is very widely used for web services (including consumer services). OAuth is designed to work over HTTP and is currently on version 2.0, which is not compatible with version 1.0. To add a little confusion, OAuth 2.0 is more of a framework and less rigid than OAuth 1.0 which means implementations may not be compatible. It is most often used for delegating access control/authorizations between services.
* *OpenID:* is a standard for federated authentication that is very widely supported for web services. It is based on HTTP with URLs used to identify the identity provider and the user/identity (e.g. identity.identityprovider.com) The current version is OpenID Connect 1.0 and it is very commonly seen in consumer services.

There are two other standards that aren't as commonly encountered but can be useful for cloud computing:

* *eXtensible Access Control Markup Language (XACML):* is a standard for defining attribute-based access controls/authorizations. It is a policy language for defining access controls at a Policy Decision Point and then passing them to a Policy Enforcement Point. It can be used with both SAML and OAuth since it solves a different part of the problem (deciding what an entity is allowed to do with a set of attributes, as opposed to handling log ins or delegation of authority). 
* *System for Cross-domain Identity Management (SCIM):* is a standard for exchanging identity information between domains. It can be used for provisioning and deprovisioning accounts in external systems and exchanging attribute information. 

> Callout and diagram- How Federated Identity Management Works: Federation involves an *identity provider* making assertions to a *relying party* after building a trust relationship. At the heart are a series of cryptographic operations to build the trust relationship and exchange credentials. A practical example is a user logging into their work network that hosts a directory server for accounts. That user then opens a browser connection to a SaaS application. Instead of logging in there are a series of behind the scenes operations where the identity provider (the internal directory server) asserts the identity of the user, that the user authenticated, and any attributes. The relying party trusts those assertions and logs the user in without the user entering any credentials. In fact, the relying party doesn't even have a username or password for that user, it relies on the identity provider to assert successful authentication. To the user they simply go to the website for the SaaS application after appear logged in, assuming they successfully authenticated with the internal directory.

This isn't to imply there aren't other techniques or standards used in cloud computing for identity, authentication and authorization. Most cloud providers, especially IaaS providers, have their own internal IAM systems which might not use any of these standards or can be connected to an organization using these standards. For example, HTTP request signing is very commonly used for authenticating REST APIs and authorization decisions are managed by internal policies on the cloud provider side. The request signing might still support SSO through SAML, or the API might be completely OAuth based, or use its own token mechanism. All are commonly encountered but most enterprise-class cloud providers offer federation support of some sort.

> Identity protocols and standards do not represent a complete solution by themselves, but they are a means to an end. The essential concepts when choosing an identity protocol are:

* No protocol is a silver bullet that solves all identity and access control problems.
* Identity protocols must be analyzed in the context of use case(s), for example Browser based Single Sign On, API keys, or mobile to cloud authentication could each lead companies to a different approach.
* The key operating assumption should be that identity is a perimeter in and of itself, just like a DMZ. So any identity protocol has to be selected and engineered from the standpoint that it can traverse risky territory and withstand malice

### Managing users and identities for cloud computing

The "identity" part of identity management focuses on the processes and technologies for registering, provisioning, propagating, managing, and deprovisioning identities. Managing identities and provisioning them in systems are problems information security has been tackling for decades. It wasn't that long ago where IT administrators needed to individually provision users in every different internal systems. Even today, with centralized directory servers and a range of standards, true Single Sign On for everything is relatively rare and users manage a set of credentials, albeit a much smaller set than in the past.

> A note on scope: The descriptions in this section are generic but do skew towards user management. The same principles apply to identities for services, devices, servers, code, and other entities but the processes and details around those can be more complex and tie tightly to application security and architectures. This domain also only includes limited discussion of all the internal identity management issues for cloud providers for the same reasons. It isn't that these areas are less important; in many cases they are more important, but they also bring a complexity that can't be fully covered within the constraints of this Guidance. 

Cloud providers and cloud consumers need to start with the fundamental decision on how to manage identities:

* Cloud providers need to nearly always support internal identities, identifiers, and attributes for users who directly access the service, while also supporting federation so organizations don't have to manually provision and manage every user in the provider's system and issue everyone separate credentials. 
* Cloud consumers need to decide where they want to manage their identities and which architectural models and technologies they want to support to integrate with cloud providers. 

As a cloud consumer you can log into a cloud provider and create all your identities in their system. This is not scalable for most organizations, which is why most turn to federation. Keep in mind there *can* be exceptions where it makes sense to keep all or some of the identities with the cloud provider isolated, such as backup administrator accounts to help debug problems with the federated identity connection.

When using federation the cloud consumer needs to determine the authoritative source that holds the unique identities they will federate. This is often an internal directory server. The next decision is to use the authoritative source as the identity provider directly, a different identity source that feeds from the authoritative source (like a directory fed from an HR system) or to integrate an *identity broker*. There are two possible architectures:

> Insert image of models here

* *Free form:* internal identity providers/sources (often directory servers) connect directly to cloud providers.
* *Hub and spoke:* internal identity providers/sources communicate with a central broker or repository that then serves as the identity provider for federation to cloud providers.
 
Directly federating internal directory servers in the free form model raises a few issues:

* The directory needs Internet access. This can be a problem depending on existing topography or may violate security policies.
* It may require users to VPN back to the corporate network before accessing cloud services.
* Depending on the existing directory server, and especially if you have multiple directory servers in different organizational silos, federating to an external provider may be complex and technically difficult.

*Identity brokers* handle federating between identity providers and relying parties (which may not always be a cloud service). They can be located on the network edge or even in the cloud to enable web-SSO. 

Identity providers don't need to be located only on-premise; many cloud providers now support cloud-based directory servers that support federation internally and with other cloud services. For example, more-complex architectures can synchronize or federate a portion of an organization's identities for an internal directory through an identity broker and then to a cloud-hosted directory that serves as an identity provider for other federated connections. 

After determining the large-scale model there are still process and architectural decisions required for any implementation:

* How to manage identities for application code, systems, devices, and other services. You may leverage the same model and standards or decide to take a different approach within cloud deployments and applications. For example, the descriptions above skew towards users accessing services but may not apply equally for services talking to services, systems or devices talking to services, or for application components within an IaaS deployment. 
* Defining the identity provisioning process and how to integrate that into cloud deployments. There may also be multiple provisioning processes for different use cases although the goal should be to have as unified a process as possible. 
	* If the organization has an effective provisioning process in place for traditional infrastructure this should ideally be extended into cloud deployments. However, if existing internal processes are problematic the organization should instead use the move to cloud as an opportunity to build a new, more effective process.
* Provisioning and supporting individual cloud providers and deployments. There should be a formal process for adding new providers into the IAM infrastructure. This includes the process of establishing any needed federation connections as well as:
	* Mapping attributes (including roles) between the identity provider and the relying party. 
	* Enabling required monitoring/logging, including identity-related security monitoring like behavioral analytics.
	* Building an entitlement matrix (discussed more in the next section).
	* Documenting any break/fix scenarios in case there is a technical failure of any of the federation (or other techniques) used for the relationship.
	* Ensuring incident response plans for potential account takeovers are in place, including takeovers of privileged accounts.
* Implementing deprovisioning or entitlement change processes for identities and the cloud provider. With federation this requires work on both sides of the connection.

Lastly, cloud providers need to determine which identity management standards they wish to support. Some providers support only federation while others support multiple IAM standards plus their own internal user/account management. Providers who serve enterprise markets will need to support federated identity, and most likely SAML. 

### Authentication and Credentials

Authentication is the process of proving or confirming an identity. In information security authentication most commonly refers to the act of a user logging in, but it also refers to essentially any time an entity proves who they are and assumes an identity. Authentication is the responsibility of the identity provider. 

The biggest impact of cloud computing on authentication is a greater need for *strong authentication* using *multiple factors*. This is for two reasons:

* Broad network access means cloud services are always accessed over the network, and often over the Internet. Loss of credentials could more-easily lead to an account takeover by an attacker since attacks aren't restricted to the local network.
* Greater use of federation for Single Sign On means one set of credentials can potentially compromise a greater number of cloud services.

Multifactor authentication (MFA) offers one of the strongest options for reducing account takeovers. It isn't a panacea, but relying on a single factor (password) for cloud services is very high risk. When using MFA with federation the identity provider can and should pass the MFA status as an attribute to the relying party.

There are multiple options for MFA, including:

* *Hard tokens* are physical devices that generate one time passwords for human entry or need to be plugged into a reader. These are the best option when the highest level of security is required.
* *Soft tokens* work similarly to hard tokens but are software applications that run on a phone or computer. Soft tokens are also an excellent option but could be compromised if the user's device is compromised, and this risk needs to be considered in any threat model.
* *Out of band Passwords* are text or other messages sent to a user's phone (usually) and are then entered like any other one time password generated by a token. Although also a good option message interception, especially with SMS, must be considered in any threat model. 
* *Biometrics* are growing as an option thanks to biometric readers now commonly available on mobile phones. For cloud services the biometric is a *local protection* that doesn't send biometric information to the cloud provider and is instead an attribute that can be sent to the provider. As such the security and ownership of the local device needs to be considered.

For customers, [FIDO](https://fidoalliance.org) is one standard that may streamline stronger authentication for consumers while minimizing friction.

### Entitlement and Access Management

Since the terms *entitlement, authorization, and access control* all overlap somewhat and are defined differently depending on the context. Although we defined them earlier in this section here is a quick review. An authorization is permission to do something — access a file or network, or perform a certain function like an API call on a particular resource. An access control allows or denies the expression of that authorization so it includes aspects like assuring the user is authenticated before allowing access. An entitlement maps identities  to authorizations and any required attributes (e.g. user x is allowed access to resource y when z attributes have designated values). We commonly refer to a map of these entitlements as an *entitlement matrix*. Entitlements are often encoded as technical *policies* for distribution and enforcement.

This is only one definition of these terms and you may see them used differently in other documents. We also use the term *access management* as the "A" half of IAM and it refers to the entire process of defining, propagating, and enforcing authorizations.

Here's a real-world cloud example. The cloud provider has an API for launching new virtual machines. That API has a corresponding authorization to allow launching new machines, with additional authorization options for what virtual network a user can launch the VM within. The cloud administrator creates an entitlement that says users in the developer group can launch virtual machines in only their project network and only if they authenticated with MFA. The group and use of MFA are attributes of the user's identity. That entitlement is written as a policy that is loaded into the cloud provider's system for enforcement.

Cloud impacts entitlements, authorizations, and access management in multiple ways:

* Cloud providers and platforms, like any other technology, will have their own set of potential authorizations specific to them. Unless the provider supports XACML (rare today) the cloud consumer will usually need to configure entitlements within the cloud platform directly.

* The cloud provider is responsible for enforcing authorizations and access controls.
* The cloud consumer is responsible for defining entitlements and properly configuring them within the cloud platform.

* Cloud platforms tend to have greater support for the *Attribute Based Access Control* model for IAM, which offer greater flexibility and security than *Role Based Access Control* model. RBAC is the traditional model for enforcing authorizations and relies on what is often a single attribute (a defined role). ABAC allows more granular and context aware decisions by incorporating multiple attributes, such as role, location, authentication method, and more. 
* ABAC is the preferred model for cloud-based access management.

* When using federation the cloud consumer is responsible for mapping attributes, including roles and groups, to the cloud provider and ensuring these are properly communicated during authentication. 
* Cloud providers are responsible for supporting granular attributes and authorizations to enable ABAC and effective security for cloud consumers.

### Privileged User Management

In terms of controlling risk, few things are more essential than privileged user management. The requirements mentioned above for strong authentication should be a strong consideration for any privileged user. In addition, account and session recoding should be implemented to drive up accountability and visibility for privileged users.

In some cases, it will be beneficial for Privileged user to sign through a separate tightly controlled system using higher levels of assurances for credential control, digital certificates, physically and logically separate access points, and/or jump hosts

## Recommendations

* Organizations should develop a comprehensive and formalized plan and processes for managing identities and authorizations with cloud services. 
* When connecting to external cloud providers use federation, if possible, to extend existing identity management. Try to minimize silos of identities in cloud providers that are not tied to internal identities.
* Consider use of identity brokers where appropriate.
* Cloud consumers are responsible for maintaining the identity provider and defining identities and attributes.
	* These should be based on an authoritative source.
	* Distributed organizations should consider using cloud-hosted directory servers when on-premise options either aren't available or do not meet requirements.
* Cloud consumers should prefer MFA for all external cloud accounts and send MFA status as an attribute when using federated authentication.
* Privileged identities should always use MFA.
* Develop an entitlement matrix for each cloud provider and project, with an emphasis on access to the metastructure and/or management plane.
* Translate entitlement matrices into technical policies when supported by the cloud provider or platform.
* Prefer ABAC over RBAC for cloud computing.
* Cloud providers should offer both internal identities and federation using open standards. 
* There are no magic protocols, pick your use cases and constraints first and then find the right protocol second.
