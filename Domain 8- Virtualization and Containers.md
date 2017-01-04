# Domain 8: Virtualization and Containers

## Introduction

* Virtualization is more than virtual machines.
	* Cloud computing is fundamentally based on pooling resources. Virtualization is the technology used to convert fixed infrastructure into pooled resources.
		* Virtualization provides the abstraction needed for resource pools, which are then managed using orchestration.
	* Major virtualization categories used by cloud computing:
		* Compute
		* Network
		* Storage
		* Containers
		* Other
* Understand the impacts of virtualization on security is fundamental to properly architecting and implementing cloud security.

## Overview 

* Virtualization abstracts resources from their underlying physical assets.
	* You can virtualize nearly anything in technology, from entire computers to networks to code. 
	* Cloud computing is fundamentally based on virtualization.
	* Many security processes are designed with the expectation of physical control over the underlying infrastructure. While this doesn't go away with cloud computing, virtualization adds two new layers for security controls.
		* Security of the virtualization technology itself. E.g. securing a hypervisor.
		* Security controls for the virtual assets, which in many cases must be implemented differently than the corresponding physical equivalent. E.g. virtual firewalls are not the same as physical firewalls, and mere abstraction of a physical firewall into a virtual machine still may not meet deployment or security requirements.
	* Virtualization security in cloud computing still follows the shared responsibility model.
		* The cloud provider will always be responsible for securing the physical infrastructure and the virtualization platform. 
		* The cloud customer is responsible for properly implementing the available virtualized security controls and understanding the underlying risks based on what is implemented and managed by the cloud provider.
* Major virtualization domains
	* Compute
		* Most commonly virtual machines, but this is changing.
		* Containers and certain kinds of serverless infrastructure also abstract compute.
			* Containers are covered in more detail in a moment.
		* Primary responsibilities of the cloud provider in compute virtualization are to enforce isolation and maintain a secure virtualization infrastructure.
			* Compute processes or memory in one virtual machine/container should not be visible to another.
			* Cloud providers should also assure customers that volatile memory is safe from unapproved monitoring.
			* The cloud provider is also responsible for securing the underlying infrastructure and the virtualization technology.
				* E.g. using patched and up to date hypervisors, properly configured, and supporting with processes to keep them up to date and secure over time.
		* Primary responsibility of the cloud consumer is to properly implement the security of what they deploy:
			* Security controls for managing their virtual infrastructure, which will vary based on the cloud platform, including:
				* Security settings, such as identity management to the virtual resources
				* Monitoring/logging
				* Use of dedicated hosting, if available, based on the security context of the resource.
					* In some situations you can specify that your assets run on hardware dedicated only to you (at higher cost). This may help meet compliance requirements and with some security needs in special cases.
			* Security controls within the virtualized resource
				* Secure configuration (e.g. a patched, updated virtual machine image)
				* Other server/container/code level security controls
		* Other general compute security concerns
			* Virtualized resources tend to be more ephemeral and change at a more rapid pace. Any corresponding security, such as monitoring, must keep up with the pace.
			* Host-level monitoring/logging may not be available, especially for serverless deployments. Alternative log methods may need to be implemented.
	* Network
		* There are multiple kinds of virtual networks, from basic VLANs to full Software Defined Networks.
			* Most cloud computing today uses SDN for virtualizing networks. VLANs are often not suitable for cloud deployments since they lack important isolation capabilities for multi-tenancy.
			* SDN abstracts the network management plane from the underlying physical infrastructure, removing many typical networking constraints.
				* For example, you can overlay multiple virtual networks, even ones that completely overlap their address ranges, over the same physical hardware with all traffic properly segregated and isolated.
				* SDNs are defined using software settings and API calls, which supports orchestration and agility.
		* Virtual networks are different than physical networks.
			* Monitoring and filtering (including firewalls) are very different.
				* Resources may communicate on a physical server without traffic spanning the physical network.
					* In some cases traffic can be virtually routed to a virtual monitoring/filtering resource/VM, but this often creates a bottleneck, increases costs, and increases complexity.
				* The cloud platform/provider may not support access for direct network monitoring.
				* With public cloud in particular, some communications between cloud services will occur on the provider's network and customer monitoring and filtering of that traffic isn't possible (and would create a security risk for the provider).
				* However, all modern cloud platforms offer built-in firewalls, which may offer advantages over corresponding physical firewalls.
					* These are software firewalls that may operate within the SDN or the hypervisor.
					* Typically offer fewer features than a modern, dedicated next-generation firewall, but these capabilities may not always be needed due to other inherent security provided by the cloud provider.
			* Virtual networks for cloud computing always support remote management; as such securing the management plane/metastructure is critical.
		* The cloud provider is primarily responsible for building a secure network infrastructure and configuring it properly.
			* Segregation and isolation of network traffic to prevent tenants from viewing traffic.
			* Disabling packet sniffing or other metadata "leaks" that could expose data or configurations between tenants.
			* Enabling firewall capabilities for the cloud consumers.
			* Detecting and preventing attacks on the underlying physical network and virtualization platform.
		* Cloud consumers are primarily responsible for properly configuring the network, especially any virtual firewalls.
			* Network architecture will play a larger role in virtual network security.
				* Since virtual networks are software constructs, use of multiple, separate virtual networks may offer extensive compartmentalization advantages not possible compared to a traditional physical network.
				* *Immutable* networks can be defined on some cloud platforms using software templates, which can help enforce known-good configurations.
			* The cloud consumer is, again, responsible for proper rights management and configuration of exposed controls.
			* When virtual firewalls and/or monitoring don't meet security needs the consumer may need to compensate with a virtual security appliance or host security agent.
	* Storage
		* Storage virtualization is already common in most organizations and storage security is discussed in more detail in *Domain 11*.
		* It is not unusual to encrypt the underlying physical storage drives to assure data isn't exposed as drives are swapped out due to failures.
			* This encryption doesn't protect data in any virtualized layers.
		* Depending on the type of storage the cloud provider may also encrypt it at the virtualization layer.
			* But this may not protect customer data from exposure to the cloud provider.
		* Thus any additional protection should be provided using the advice in Domain 11.				
* Containers
	* Containers are highly portable code execution environments. A full discussion is beyond the scope of this guidance.
		* They can be built directly on tip of physical servers, or run on virtual machines.
		* They always include three key components:
			* The execution environment (the container).
			* An orchestration and scheduling controller (which can be a collection of multiple tools).
			* A repository for the container images or code to execute.
	* Regardless of platform, container security includes:
		* Assuring the security of the underlying physical infrastructure (compute, network, storage).
		* Assuring the security of the management plane: the orchestrator and scheduler.
		* Properly securing the image repository.
		* Building security into the tasks/code running inside the container. 
	* Containers are rapidly evolving which complicates some aspects of security but doesn't mean they are inherently insecure.
		* Containers don't necessarily provide full security isolation. However, they do provide task segregation. 
			* Virtual machines typically *do* provide security isolation.
			* Thus you can use put tasks of equivalent security levels on the same physical or virtual hosts to provide greater security isolation.
		* Container management systems and image repositories have different security capabilities based on which products you use.
			* Security should learn and understand the capabilities of the products they need to support.
			* Products should, at a minimum, support role based access controls and strong authentication.
		* A deep understanding of container security relies on a deep understanding of operating system internals, such as namespaces, network port mapping, memory, and storage access.
			* Different host operating systems and container technologies offer different security capabilities. This assessment should be included in any container platform selection process.
		* One key area to secure is which images/tasks/code are allowed into the execution environment.
			* A secure repository with proper container management and scheduling is critical.

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
			* Implement essential perimeter security defenses to protect the underlying networks from attack and to, wherever possible, detect and prevent attacks against consumers at the physical and any virtual network layers they can't direct protect themselves.
			* Assure isolation between virtual networks, even if those networks are all controlled by the same consumer.
				* Unless the consumer deliberately connects the separate virtual networks.
			* Implement internal security controls and policies to prevent both modification of consumer networks and monitoring of traffic without approval or outside contractual agreements.
		* Storage
			* Encrypt any underlying physical storage, if it is not already encrypted at another level, to prevent data exposure during drive replacements.
			* Isolate encryption from data management functions to prevent unapproved access to customer data.
* Cloud consumers should:
	* Ensure they understand the capabilities offered by their cloud providers and any security gaps.
	* Properly configure virtualization services in accordance with the guidance from the cloud provider and other industry best practices.
		* The bulk of fundamental virtualization security falls on the cloud provider which is why most of the security recommendations for cloud consumers is covered in the other domains of this Guidance.
	* For containers:
		* Understand the security isolation capabilities of the chosen container platform and underlying operating system and choose the appropriate configuration.
		* Use physical or virtual machines to provide container isolation and group containers of the same security contexts on the same physical and/or virtual hosts.
		* Ensure only approved, known, secure container images or code can be deployed.
		* Appropriately secure the container orchestration/management and scheduler software stack(s).
		* Implement appropriate role based access controls and strong authentication for all container and repository management.