# Domain 8: Virtualization and Containers

## Introduction

Virtualization isn't merely a tool for creating virtual machines—it's the core technology for enabling cloud computing. We use virtualization all throughout computing, from full operating virtual machines to virtual execution environments like the Java Virtual Machine, as well as in storage, networking, and beyond.

Cloud-computing is fundamentally based on pooling resources, and virtualization is the technology used to convert fixed infrastructure into these pooled resources. Virtualization provides the *abstraction* needed for resource pools, which are then managed using orchestration.

As mentioned, virtualization covers an extremely wide range of technologies; essentially anytime we create an abstraction, we're using virtualization. For cloud-computing we tend to focus on those specific aspects of virtualization used to create our resource pools, especially:

* Compute
* Network
* Storage
* Containers

The aforementioned aren't the only categories of virtualization, but they are the ones most relevant to cloud-computing.

Understanding the impacts of virtualization on security is fundamental to properly architecting and implementing cloud security. Virtual assets provisioned from a resource pool may *look* just like the physical assets they replace, but that look and feel is really just a tool to help us better understand (through familiarity) and manage what we see. It's also a useful way to leverage existing technologies, like operating systems, without having to completely rewrite them from scratch. Underneath, these virtual assets work completely differently from the resources they are abstracted from.

## Overview 

At its most basic, virtualization abstracts resources from their underlying physical assets. You can virtualize nearly anything in technology, from entire computers to networks to code.  As mentioned in the introduction, cloud-computing is fundamentally based on virtualization. It's how we abstract resources to create pools. Without virtualization, there is no cloud. 

Many security processes are designed with the expectation of physical control over the underlying infrastructure. While this doesn't go away with cloud-computing, virtualization adds two new layers for security controls:

* *Security of the virtualization technology itself.* E.g. securing a hypervisor.
* *Security controls for the virtual assets.* In many cases, this must be implemented differently than it would be in the corresponding physical equivalent. For example, as discussed in Domain 7, virtual firewalls are not the same as physical firewalls, and mere abstraction of a physical firewall into a virtual machine still may not meet deployment or security requirements.

Virtualization security in cloud computing still follows the shared responsibility model. The cloud provider will always be responsible for securing the physical infrastructure and the virtualization platform itself. Meanwhile, the cloud customer is responsible for properly implementing the available virtualized security controls and understanding the underlying risks, based on what is implemented and managed by the cloud provider. For example, deciding when to encrypt virtualized storage, properly configuring the virtual network and firewalls, or deciding when to use dedicated hosting vs. a shared host. 

Since many of these controls span into other areas of cloud security, such as data security, we try to focus on the virtualization-specific concerns in this domain. The lines aren't always clear, and the bulk of cloud security controls are covered more deeply in the other domains of this Guidance. Domain 7: Infrastructure Security focuses extensively on virtual networks and workloads.

### Major virtualization categories relevant to cloud computing

#### Compute

Compute virtualization abstracts the running of code (including operating systems) from the underlying hardware. Instead of running directly on the hardware, the code runs on top of an abstraction layer that enables more flexible usage, such as running multiple operating systems on the same hardware (virtual machines). This is a simplification and we recommend further research into virtual machine managers and hypervisors if you are interested in learning more. 

Compute most commonly refers to virtual machines, but this is quickly changing, in large part due to ongoing technology evolution and adoption of containers.

Containers and certain kinds of serverless infrastructure also abstract compute. These are different abstractions to create code execution environments, but they don't abstract a full operating system as we do with a virtual machine. (Containers are covered in more detail below.)

##### Cloud Provider Responsibilities

The primary security responsibilities of the cloud provider in compute virtualization are to enforce *isolation* and maintain a *secure virtualization infrastructure*.

* *Isolation* ensures that compute processes or memory in one virtual machine/container should not be visible to another. It is how we separate different tenants, even when they are running processes on the same physical hardware. 

* The cloud provider is also responsible for securing the *underlying infrastructure and the virtualization technology* from external attack or internal misuse. This means using patched and up-to-date hypervisors, properly configured, and supported with processes to keep them up to date and secure over time. The inability to patch hypervisors across a cloud deployment could create a fundamentally insecure cloud when a new vulnerability in the technology is discovered. 

Cloud providers should also support secure use of virtualization for cloud consumers. This means creating a secure chain of processes from the images (or other source) used to run the virtual machine all the way through a boot process with security and integrity. This ensures that tenants cannot launch machines based on images that they shouldn't have access to, such as those belonging to another tenant, and that a running virtual machine (or other process) is the one the customer expects to be running. 

In addition, cloud providers should assure customers that volatile memory is safe from unapproved monitoring, since important data could be exposed if another tenant, a malicious employee, or even an attacker is able to access running memory.

##### Cloud Consumer Responsibilities

Meanwhile, the primary responsibility of the cloud consumer is to properly implement the security of whatever they deploy within the virtualized environment. Since the onus of compute virtualization security is on the provider, the customer tends to have only a few security options relating directly to the virtualization of the workload. There is quite a bit more to securing workloads, and those are covered in Domain 7. 

That said, there are still some virtualization-specific differences that the cloud consumer can address in their security implementation. Firstly, the security controls for *managing* their virtual infrastructure, which will vary based on the cloud platform, and often include:

* *Security settings, such as identity management, to the virtual resources.* This is not the identity management *within* the resource, such as the operating system login credentials, but the identity management of who is allowed to access the cloud management of the resource, such as stopping or changing the configuration of a virtual machine. See Domain 6 for specifics on management plane security.

* *Monitoring and logging.* Domain 7 covers monitoring and logging of workloads, including how to handle system logs from virtual machines of containers, but the cloud platform will likely offer additional logging and monitoring at the virtualization level. This can include that status of a virtual machine, management events, performance, etc.

* *Image asset management.* Cloud compute deployments are based on master images—be it a virtual machine, container, or other code—that are then run in the cloud. This is often highly automated and results in a larger number of images to base assets on, compared to traditional computing master images. Managing these—including which meet security requirements, where they can be deployed, and who has access to them—is an important security responsibility.
				
* *Use of dedicated hosting*, if available, based on the security context of the resource. In some situations you can specify that your assets run on hardware dedicated only to you (at higher cost) even on a multi-tenant cloud. This may help meet compliance requirements or satisfy security needs in special cases where sharing hardware with another tenant is considered a risk. 
	
Secondly, security controls *within* the virtualized resource:

* This includes all the standard security for the workload, be it a virtual machine, container, or application code. These are well covered by standard security best practices and the additional guidance in Domain 7.

* Of particular concern is ensuring deployment of only secure configurations (e.g. a patched, updated virtual machine image). Due to the automation of cloud computing it is easy to deploy older configurations that may not be patched or properly secured. 
		
Other general compute security concerns:

* Virtualized resources tend to be more ephemeral and change at a more rapid pace. Any corresponding security, such as monitoring, must keep up with the pace. Again, the specifics are covered in more depth in Domain 7.

* Host-level monitoring/logging may not be available, especially for serverless deployments. Alternative log methods may need to be implemented. For example, in a serverless deployment, you are unlikely to see system logs of the underlying platform and should offset by writing more robust application logging into your code.
	
### Network

There are multiple kinds of virtual networks, from basic Virtual Local Area Networks (VLANs) to full Software Defined Networks (SDN). As a core of cloud infrastructure security these are covered both here and in Domain 7. 

To review, most cloud-computing today use SDN for virtualizing networks. (VLANs are often not suitable for cloud deployments since they lack important isolation capabilities for multi-tenancy.)

SDN abstracts the network management plane from the underlying physical infrastructure, removing many typical networking constraints. For example, you can overlay multiple virtual networks, even ones that completely overlap their address ranges, over the same physical hardware, with all traffic properly segregated and isolated. SDNs are also defined using software settings and API calls, which supports orchestration and agility.

Virtual networks are quite different than physical networks. They run on physical networks, but abstraction allows for deep modification on networking behavior in ways that impacts many security processes and technologies.

#### Monitoring and Filtering

In particular, monitoring and filtering (including firewalls) change extensively due to the differences in how packets move around the virtual network. Resources may communicate on a physical server without traffic spanning the physical network. If two virtual machines are located on the same physical machine there is no reason to route network traffic off the box and onto the network. Thus they can communicate directly, and monitoring and filtering tools inline on the network (or attached to the routing/switching hardware) will never see the traffic. 

To compensate, you can route traffic to a virtual network monitoring or filtering tool on the same hardware (including a virtual machine version of a network security product). You can also bridge all network traffic back out to the network, or route it to a virtual appliance on the same virtual network. Each of these approaches has drawbacks since it creates bottlenecks and less-efficient routing. 

The cloud platform/provider may not support access for direct network monitoring. Public cloud providers rarely allow full packet network monitoring to customers, due to the complexity (and cost). Thus you can't assume you will ever have access to raw packet data unless you collect it yourself in the host or using a virtual appliance.

With public cloud in particular, some communications between cloud services will occur on the provider's network; customer monitoring and filtering of that traffic isn't possible (and would create a security risk for the provider). For example, if you connect a serverless application to the cloud provider's object storage, database platform, message queue, or other PaaS product, this traffic would run natively on the provider's network, not necessarily within the customer-managed virtual network. As we move out of simple infrastructure virtualization the concept of a customer-managed network begins to fade.

However, all modern cloud platforms offer built-in firewalls, which may offer advantages over corresponding physical firewalls. These are software firewalls that may operate within the SDN or the hypervisor. They typically offer fewer features than a modern, dedicated next-generation firewall, but these capabilities may not always be needed due to other inherent security provided by the cloud provider.

#### Management Infrastructure

Virtual networks for cloud-computing always support remote management and, as such, securing the management plane/metastructure is critical. At times it is possible to create and destroy entire complex networks with a handful of API calls or a few clicks on a web console. 

##### Cloud Provider Responsibilities

The cloud provider is primarily responsible for building a secure network infrastructure and configuring it properly.  The absolute top security priority is segregation and isolation of network traffic to prevent tenants from viewing another's traffic. This is the most foundational security control for any multi-tenant network.

The provider should disable packet sniffing or other metadata "leaks" that could expose data or configurations between tenants. Packet sniffing, even within a tenant's own virtual networks, should also be disabled to reduce that ability of an attacker to compromise a single node and use it to monitor the network, as is common on non-virtualized networks. Tagging or other SDN-level metadata should also not be exposed outside the management plane or the compromise of a host could be used to span into the SDN itself.

All virtual networks should enable built-in firewall capabilities for cloud consumers without the need for host firewalls or external products. The provider is also responsible for detecting and preventing attacks on the underlying physical network and virtualization platform. This includes perimeter security of the cloud itself.

##### Cloud Consumer Responsibilities

Cloud consumers are primarily responsible for properly configuring their deployment of the virtual network, especially any virtual firewalls.

Network architecture can play a larger role in virtual network security since we aren't constrained by physical connections and routing. Since virtual networks are software constructs, use of multiple, separate virtual networks may offer extensive compartmentalization advantages not possible compared to a traditional physical network. You can run every application stack in its own virtual network, which dramatically reduces the attack surface if a malicious actor gains a foothold. An equivalent architecture on a physical network is cost prohibitive.

*Immutable* networks can be defined on some cloud platforms using software templates, which can help enforce known-good configurations. The entire known-good state of the network can be defined in a template, instead of having to manually configure all the settings. Aside from the ability to create multiple networks with a secure baseline these can also be used to detect, and in some cases revert, deviations from known good states.

The cloud consumer is, again, responsible for proper rights management and configuration of exposed controls in the management plane. When virtual firewalls and/or monitoring don't meet security needs, the consumer may need to compensate with a virtual security appliance or host security agent. This falls under cloud infrastructure security and is covered in depth in Domain 7.

#### Cloud Overlay Networks

Cloud overlay networks are a special kind of WAN virtualization technology for created networks that span multiple "base" networks. For example, an overlay network could span physical and cloud locations or multiple cloud networks, perhaps even on different providers. A full discussion is beyond the scope of this Guidance and the same core security recommendations apply.

### Storage

Storage virtualization is already common in most organizations (SAN and NAS are both common forms of storage virtualization) and storage security is discussed in more detail in Domain 11.

When virtualizing storage it is not unusual to encrypt the underlying physical storage drives to assure that data isn't exposed as drives are swapped out due to hardware failures. Most virtualized storage is *durable* and keeps multiple copies of data in different locations so that drive failures are less likely to result in data loss. Encrypting those drives reduces the concern that swapping out a drive, which is a very frequent activity, could result in data exposure.

However, this encryption doesn't protect data in any virtualized layers; it only protects the data at physical storage. Depending on the type of storage the cloud provider may also (or instead) encrypt it at the virtualization layer, but this may not protect customer data from exposure to the cloud provider. Thus any additional protection should be provided using the advice in Domain 11.				

### Containers

Containers are highly portable code execution environments. To simplify, a virtual machine is a complete operating system, all the way down to the kernel. A container, meanwhile, is a virtual execution environment that features an isolated user space, but uses a shared kernel. A full discussion is beyond the scope of this guidance and [you can read more about software containers at this Wikipedia entry](https://en.wikipedia.org/wiki/Operating-system-level_virtualization).

Such containers can be built directly on top of physical servers or run on virtual machines. Current implementations rely on an existing kernel/operating system, which is why they can run inside a virtual machine even if nested virtualization is not supported by the hypervisor. (Software containers rely on a completely different technology for hypervisors.)

Software container systems always include three key components:
* The execution environment (the container).
* An orchestration and scheduling controller (which can be a collection of multiple tools).
* A repository for the container images or code to execute.

Together these are the place to run things, the things to run, and the management system to tie them together.

Regardless of the technology platform, container security includes:
	
* *Assuring the security of the underlying physical infrastructure (compute, network, storage).* This is no different than any other form of virtualization, but it now extends into the underlying operating system where the container execution environment runs.

* *Assuring the security of the management plane*, which in this case are the orchestrator and the scheduler.

* *Properly securing the image repository.* The image repository should be in a secure location with appropriate access controls configured. This is to both to prevent loss or unapproved modification of container images and definition files, as well as leaks of sensitive data through unapproved access to the files. Containers run so easily that it's also important that images are only able to deploy in the right security context.

* *Building security into the tasks/code running inside the container.* It's still possible to run vulnerable software inside a container, and in some cases this could expose the shared operating system or data from other containers. For example, it is possible to configure some containers to allow root file system access, not merely access to the container's data on the file system. Allowing too much network access is also a possibility. These are all specific to the particular container platform and thus requires securely configuring both the container environment *and* the images/container configurations themselves.

Containers are rapidly evolving, which complicates some aspects of security, but doesn't mean that they are inherently insecure.

Containers don't necessarily provide full security isolation. However, they do provide task segregation. However, virtual machines typically *do* provide security isolation. Thus you can put tasks of equivalent security context on the same set of physical or virtual hosts in order to provide greater security segregation.

Container management systems and image repositories also have different security capabilities, based on which products you use. Security should learn and understand the capabilities of the products they need to support. Products should, at a minimum, support role-based access controls and strong authentication. They should also support secure configurations, such as isolating file system, process, and network access.

A deep understanding of container security relies on a deep understanding of operating system internals, such as namespaces, network port mapping, memory, and storage access.

Different host operating systems and container technologies offer different security capabilities. This assessment should be included in any container platform selection process.

One key area to secure is which images/tasks/code are allowed into a particular execution environment. A secure repository with proper container management and scheduling will enable this.

## Recommendations

* Cloud providers should:	
	* Inherently secure any underlying physical infrastructure used for virtualization.
	* Focus on assuring security isolation between tenants.
	* Provide sufficient security capabilities at the virtualization layers to allow cloud consumers to properly secure their assets.
	* Strongly defend the physical infrastructure and virtualization platforms from attack or internal compromise.
	* Implement all customer-managed virtualization features with a secure by default configuration.
	* Specific priorities:
		* Compute
			* Use secure hypervisors and implement a patch management process to keep them up to date.
			* Configure hypervisors to isolate virtual machines from each other.
			* Implement internal processes and technical security controls to prevent admin/non-tenant access to running VMs or volatile memory.
		* Network
			* Implement essential perimeter security defenses to protect the underlying networks from attack and to, wherever possible, detect and prevent attacks against consumers at the physical level and at any virtual network layers that they can't directly protect themselves.
			* Assure isolation between virtual networks, even if those networks are all controlled by the same consumer.
				* Unless the consumer deliberately connects the separate virtual networks.
			* Implement internal security controls and policies to prevent both modification of consumer networks and monitoring of traffic without approval or outside contractual agreements.
		* Storage
			* Encrypt any underlying physical storage, if it is not already encrypted at another level, to prevent data exposure during drive replacements.
			* Isolate encryption from data management functions to prevent unapproved access to customer data.

* Cloud consumers should:
	* Ensure they understand the capabilities offered by their cloud providers as well as any security gaps.
	* Properly configure virtualization services in accordance with the guidance from the cloud provider and other industry best practices.
		* The bulk of fundamental virtualization security falls on the cloud provider, which is why most of the security recommendations for cloud consumers is covered in the other domains of this Guidance.
	* For containers:
		* Understand the security isolation capabilities of the chosen container platform and underlying operating system and choose the appropriate configuration.
		* Use physical or virtual machines to provide container isolation and group containers of the same security contexts on the same physical and/or virtual hosts.
		* Ensure that only approved, known, secure container images or code can be deployed.
		* Appropriately secure the container orchestration/management and scheduler software stack(s).
		* Implement appropriate role-based access controls and strong authentication for all container and repository management.