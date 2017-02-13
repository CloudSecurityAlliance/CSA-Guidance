
# Domain 7: Infrastructure Security  

## Introduction

Infrastructure security is the foundation for operating securely in the cloud. "Infrastructure" is the glue of computers and networks that we build everything on top of. For the purposes of this Guidance we start with compute and networking security, which also encompass workload and hybrid cloud. Although storage security is also core to infrastructure, it is covered in full depth in Domain 11: Data Security and Encryption. This domain also includes the fundamentals for private cloud computing. It *does not* include all the components of traditional data center security that are already well covered by existing standards and guidance.

Infrastructure security encompasses the lowest layers of security, from physical facilities through the consumer's configuration and implementation of infrastructure components. These are the fundamental components that everything else in the cloud is built from, including compute (workload), networking, and storage security.

For purposes of the CSA Guidance we are focusing on cloud-specific aspects of infrastructure security. There are already incredibly robust bodies of knowledge and industry standards for data center security that cloud providers and private cloud deployments should reference. Consider this Guidance a layer on top of those extensive and widely available materials. Specifically, this domain discusses two aspects: *cloud considerations for the underlying infrastructure*, and *security for virtual networks and workloads*. 

## Overview

In cloud computing there are two macro layers to infrastructure:

* The fundamental resources pooled together to create a cloud. This is the raw, physical and logical compute (processors, memory, etc.), networks, and storage used to build the cloud's resource pools. For example, this includes the security of the networking hardware and software used to create the network resource pool.

* The virtual/abstracted infrastructure managed by a cloud consumer. That's the compute, network, and storage assets that they use *from* the resource pools. For example, the security of the virtual network, as defined and managed by the cloud consumer.

The information and advice in this domain primarily focuses on the second macro layer, infrastructure security for the cloud consumer. Infrastructure security that's more fundamental for cloud providers, including those who manage private clouds, is well aligned with existing security standards for data centers.

## Cloud network virtualization

All clouds utilize some form of virtual networking to abstract the physical network and create a network resource pool. Typically the cloud consumer provisions desired networking resources from this pool, which can then be configured within the limits of the virtualization technique used. For example, some cloud platforms only support allocation of IP addresses within particular subnets, while others allow the cloud consumer the capability to provision entire Class B virtual networks and completely define the subnet architecture.

If you are a cloud provider (including managing a private cloud), physical segregation of networks is important for both operational and security reasons. We most commonly see at least three different networks:

* The service network for communications between virtual machines and the Internet. This builds the network resource pool for the cloud consumers.
* The storage network to connect virtual storage to virtual machines. 
* A management network for management and API traffic.

This isn't the only way to build out a private cloud network architecture, but it is a common baseline, especially for private clouds that don't deal with the massive scale of public cloud providers but still need to balance performance and security.

There are two major categories of network virtualization commonly seen in cloud computing today:
 
* VLANs (Virtual LANs):

VLANs leverage existing network technology implemented in most network hardware. VLANs are extremely common in enterprise networks, even without cloud computing. They are designed for use in single-tenant networks (enterprise data centers) to separate different business units, functions, etc. (like guest networks). VLANs are not designed for cloud-scale virtualization or security and shouldn't be considered, on their own, an effective security control for isolating networks. They are also never a substitute for physical network segregation.

* Software Defined Networking (SDN)

A more complete abstraction layer on top of networking hardware, SDNs decouple the network control plane from the data plane (you can [read more on SDN principles at the Wikipedia entry](https://en.wikipedia.org/wiki/Software-defined_networking). This allows us to abstract networking from the traditional limitations of a LAN. 

There are multiple implementations, including standards-based and proprietary options. Depending on the implementation, SDN can offer much higher flexibility and isolation. For example, multiple segregated overlapping IP ranges for a virtual network on top of the same physical network. Implemented properly, and unlike standard VLANs, SDNs provide effective security isolation boundaries. SDNs also typically offer software definition of arbitrary IP ranges, allowing customers to better extend their existing networks into the cloud. If the customer needs the 10.0.0.0/16 CIDR range an SDN can support it, regardless of the underlying network addressing. It can typically even support multiple customers using the same internal networking IP address blocks.

On the surface, an SDN may look like a regular network to a cloud consumer, but being a more complete abstraction will function very differently beneath the surface. The underlying technologies and the management of the SDN will look nothing like what the cloud consumer accesses, and will have quite a bit more complexity. For example, an SDN may use packet encapsulation so that virtual machines and other "standard" assets don't need any changes to their underlying network stack. The virtualization stack takes packets from standard operating systems connecting through a virtual network interface, then encapsulates the packets to move them around the actual network. The virtual machine doesn't need to have any knowledge of the SDN beyond a compatible virtual network interface, which is provided by the hypervisor. 

## How security changes with cloud networking

The lack of direct management of the underlying physical network changes common network practices for the cloud consumer and provider. The most commonly used network security patterns rely on control of the physical communication paths and insertion of security appliances. This isn't possible for cloud customers, since they only operate at a virtual level.

Customer security tools need to rely on an in-line virtual appliance, or a software agent installed in instances. This creates either a chokepoint or increases processor overhead, so be sure you really need that level of monitoring before implementing. Some cloud providers may offer some level of built-in network monitoring (and you have more options with private cloud platforms) but this isn't typically to the same degree as when sniffing a physical network.

### Challenges of virtual appliances

Since physical appliances can't be inserted (except by the cloud provider) they must be replaced by virtual appliances if still needed, and if the cloud network supports the necessary routing. This brings the same concerns as inserting virtual appliances for network monitoring:

* Virtual appliances thus become bottlenecks, since they cannot fail open, and must intercept all traffic. 

* Virtual appliances may take significant resources and increase costs to meet network performance requirements.

* When used, virtual appliances should support auto-scaling to match the elasticity of the resources they protect. Depending on the product, this could cause issues if the vendor does not support elastic licensing compatible with auto-scaling.

* Virtual appliances should also be aware of operating in the cloud, as well as the ability of instances to move between different geographic and availability zones. The *velocity* of change in cloud networks is higher than that of physical networks and tools need to be designed to handle this important difference.  

* Cloud application components tend to be more distributed to improve resiliency and, due to auto-scaling, virtual servers may have shorter lives and be more prolific. This changes how security policies need to be designed.
	* This induces that very high rate of change that security tools must be able to manage (e.g. Servers with a lifespan of less than an hour).
	* IP addresses will change far more quickly than on a traditional network, which security tools must account for. Ideally they should identify assets on the network by a unique ID, not an IP address or network name.
	* Assets are less likely to exist at static IP addresses. Different assets may share the same IP address within a short period of time. Assets within a single application tier will often be located on multiple subnets for resiliency, further complicating IP-based security policies. On the upside, cloud architectures skew towards fewer services per server, which improves your ability to define restrictive firewall rules. Instead of a stack of services on a single virtual machine—as on physical servers where you need to maximize the capital investment in the hardware—it is common to run a much smaller set of services, or even a single service, on a virtual machine.
        
### SDN security benefits       
        
On the positive side, software defined networks enable new types of security controls, often making it an overall gain for network security:

* Isolation is easier. 
It becomes possible to build out as many isolated networks as you need without constraints of physical hardware. For example, if you run multiple networks with the same CIDR address blocks there is no logical way they can directly communicate, due to addressing conflicts. This is an excellent way to segregate applications and services of different security contexts.  
	
* SDN firewalls (e.g, security groups) can apply to assets based on more flexible criteria than hardware-based firewalls, since they aren't limited based on physical topology. (Note that this is true of many types of software firewalls, but is distinct from hardware firewalls). SDN firewalls are typically policy sets that define ingress and egress rules that can apply to single assets or groups of assets, regardless of network location (within a given virtual network). For example, you can create a set of firewall rules that apply to any asset with a particular tag. Keep in mind this gets slightly difficult to discuss, since different platforms use different terminology and have different capabilities to support this kind of capability, so we are trying to keep things at a conceptual level.
	* Combined with the cloud platform's orchestration layer, this enables very dynamic and granular combinations and policies with less management overhead than the equivalent using a traditional hardware or host-based approaches. For example, if virtual machines in an auto-scale group are automatically deployed in multiple subnets and load balanced across them, then you can create a firewall ruleset that applies to these instances, regardless of their subnet or IP address. It is a key enabling feature of secure cloud networks that use architectures quite differently from traditional computing.
	* Default deny is often the starting point, and you are required to open connections from there, which is the opposite of most physical networks.
		* Think of it as the granularity of a host firewall with the better manageability of a network appliance. Host firewalls have two issues; they are difficult to manage at scale, and if the system they are on is compromised they are easy to alter and disable. On the other hand it is cost-prohibitive to route all internal traffic, even between peers on a subnet, through a network firewall. Software firewalls, such as security groups, are managed outside a system yet apply to each system, with not additional hardware costs or complex provisioning needed. Thus it is trivial to do things like isolate every single virtual machine on the same virtual subnet.
		* As briefly mentioned above, firewall rules can be based on other criteria, such as tags. Note that while the potential is there, the actual capabilities depend on the platform. Just because a cloud network is SDN-based doesn't mean it actually conveys any security benefits. 
		* Many network attacks are eliminated by default (depending on your platforms) such as ARP spoofing and other lower level exploits, beyond merely eliminating sniffing. This is due to the inherent nature of the SDN and application of more software-based rules and analysis in moving packets.
        * It is possible to encrypt packets as they are encapsulated.
		* As with security groups, other routing and network design can be dynamic and tied to the cloud's orchestration layer, such as bridging virtual networks or connecting to internal PaaS services.
		* Additional security functions can potentially be added natively.

### Additional Considerations for Cloud Providers or Private Clouds

Providers must maintain the core security of the physical/traditional networks that the platform is built on. A security failure at the root network will likely compromise the security of all customers. And this security must be managed for arbitrary communications and multiple tenants, some of which must be considered adversarial.

It is absolutely critical to maintain segregation and isolation for the multi-tenant environment. There will thus be additional overhead to properly enable, configure, and maintain the SDN security controls. While an SDN is more likely to provide needed isolation once it is up and running, it is important to take the extra time to get everything set up properly in order to handle potentially hostile tenants. We aren't saying your users are necessarily hostile, but it is safe to assume that, at some point, something on the network will be compromised and used to further an attack.

Providers must also expose security controls to the cloud consumers so they can properly configure and manage their network security.

Finally, providers are responsible for implementing perimeter security that protects the environment, but minimizes impact on customer workloads. For example, DDoS and baseline IPS to filter out hostile traffic before it affects the cloud's consumers.

### Hybrid cloud considerations

As mentioned in Domain 1, hybrid clouds connect an enterprise private cloud or data center to a public cloud provider, typically using either a dedicated WAN link or VPN. Ideally the hybrid cloud will support arbitrary network addressing to help seamlessly extend the cloud consumer's network. If the cloud uses the same network address range as your on-premises assets, it is effectively unusable. 
	
The hybrid connection may reduce the security of the cloud network if the private network isn't at an equivalent security level. If you run a flat network in your data center, with minimal segregation from your employee's systems, someone could compromise an employee's laptop and then use that to scan your entire cloud deployment over the hybrid connection.  A hybrid connection shouldn't effectively flatten the security of both networks. Separation should be enforced via routing, access controls, and even firewalls or additional network security tools between the two networks.

For management and security reasons it is typically preferable to minimize hybrid connections. Connecting multiple disparate networks is complex, especially when one of those networks is software-defined and the other limited by hardware. Hybrid connections are often still necessary, but don't assume they are needed. They may increase routing complexity, can reduce the ability to run multiple cloud networks with overlapping IP ranges, and complicate security on both sides, due to the need to harmonize security controls.

One emerging architecture for hybrid cloud connectivity is "bastion" or "transit" virtual networks:

* This scenario allows you to connect multiple, different cloud networks to a data center using a single hybrid connection. The cloud consumer builds a dedicated virtual network for the hybrid connection and then peers any other networks through the designated bastion network.

* Second-level networks connect to the data center through the bastion network, but since they aren't peered to each other they can't talk to each other and are effectively segregated. Also, you can deploy different security tools, firewall rulesets, and Access Control Lists in the bastion network to further protect traffic in and out of the hybrid connection.

## Cloud compute and workload security

A workload is a unit of processing, which can be in a virtual machine, a container, or other abstraction. Workloads always run somewhere on a processor and consume memory. Workloads includes a very diverse range of processing tasks, which range from traditional applications running in a virtual machine on a standard operating system, to GPU- or FPGA-based specialized tasks. Nearly every one of these options is supported in some form in cloud computing.

> It's important to remember that every cloud workload runs on a hardware stack, and the integrity of this hardware is absolutely critical for the cloud provider to maintain. Different hardware stacks also support different execution isolation and chain of trust options. This can include hardware-based supervision and monitoring processes that run outside the main processors, secure execution environments, encryption and key management enclaves, and more. The range and rapidly changing nature of these options exceeds our ability to provide proscriptive guidance at this time, but in a general sense there are potentially very large gains in security by selecting and properly leveraging hardware with these advanced capabilities.

There are multiple compute abstraction types, each with differing degrees of segregation and isolation:
                
* Virtual machines
Virtual machines are the most well known form of compute abstraction, and are offered by all IaaS providers. They are commonly called *instances* in cloud computing since they are created (or cloned) off a base image. The Virtual Machine Manager (hypervisor) abstracts an operating system from the underlying hardware. Modern hypervisors can tie into underlying hardware capabilities now commonly available on standard servers (and workstations) to reinforce isolation while supporting high performance operations.

Virtual machines are potentially open to certain memory attacks, but this is increasingly difficult due to ongoing hardware and software enhancements to reinforce isolation. VMs on modern hypervisors are generally an effective security control, and advances in hardware isolation for VMs and secure execution environments continue to improve these capabilities.

* Containers
Containers are code execution environments that run within an operating system (for now), sharing and leveraging resources of that operating system. While a VM is a full abstraction of an operating system, a container is a constrained place to run segregated processes while still utilizing the kernel and other capabilities of the base OS. Multiple containers can run on the same virtual machine or be implemented without the use of VMs at all and run directly on hardware. The container provides code running inside a restricted environment with only access to the processes and capabilities defined in the container configuration. This allows containers to launch incredibly rapidly, since they don't need to boot an operating system or launch many (sometimes any) new services; the container only needs access to already-running services in the host OS and some can launch in milliseconds.

Containers are newer, with differing isolation capabilities that are very platform-dependent. They are also evolving quickly with different management systems, underlying operating systems, and container technologies. We cover containers in more depth in Domain 8.

* Platform-based workloads
This is a more complex category that covers workloads running on a shared platform that *aren't* virtual machines or containers, such as logic/procedures running on a shared database platform. Imagine a stored procedure running inside a multi-tenant database, or a machine learning job running on a machine learning Platform as a Service. Isolation and security are totally the responsibility of the platform provider, although the provider may expose certain security options and controls.

* Serverless computing
Serverless is a broad category that refers to any situation where the cloud consumer doesn't manage any of the underlying hardware or virtual machines, and just accesses exposed functions. For example, there are serverless platforms for directly executing application code. Under the hood these still utilize capabilities like containers, virtual machines, or specialized hardware platforms. From a security perspective serverless is merely a combined term that covers containers and platform-based workloads, where the cloud provider manages all the underlying layers.

### How cloud changes workload security

Any given processor and memory will nearly always be running multiple workloads, often from different tenants. Multiple tenants will likely share the same physical compute node, and there is a range of segregation capabilities on different hardware stacks. The burden to maintain workload isolation is on the cloud provider and should be one of their top priorities.

In some environments dedicated/private tenancy is possible, but typically at a higher cost. With this model only designated workloads run on a designated physical server. Costs increase in public cloud as a consumer since you are taking hardware out of the general resource pool, but also in private cloud, due to less efficient use of internal resources.

Cloud consumers rarely get to control where a workload physically runs, regardless of deployment model, although some platforms do support designating particular hardware pools or general locations to support availability, compliance, and other requirements.
           
### Immutable workloads enable security

Auto-scaling and containers, by nature, work best when you run instances launched dynamically based on an image; those instances can be shut down when no longer needed for capacity without breaking an application stack. This is core to the elasticity of compute in the cloud. Thus you no longer patch or make other changes to a running workload, since that wouldn't change the image, and thus new instances would be out of sync with whatever manual changes you make on whatever is running. We call these virtual machines *immutable*.

To reconfigure or change an immutable instance you update the underlying image, then rotate the new instances by shutting down the old ones and running the new ones in their place. 

There are degrees of immutable. The pure definition is fully replacing running instances with a new image. However, some organizations only push new images to update the operating system and use alternative deployment techniques to push code updates into running virtual machines. While technically not completely immutable, since the instance changes, these pushes still happen completely through automation and no one ever manually logs in to running systems to make local changes.

Immutable workloads enable significant security benefits:

* You no longer patch running systems or worry about dependencies, broken patch processes, etc. You replace them with a new gold master.

* You can, and should, disable remote logins to running workloads (if logins are even an option). This is an operational requirement to prevent changes that aren't consistent across the stack, which also has significant security benefits.

* It is much faster to roll out updated versions, since applications must be designed to handle individual nodes going down (remember, fundamental to any auto-scaling). You are less constrained by the complexity and fragility of patching a running system. Even if something breaks, you just replace it.

* It is easier to disable services and whitelist applications/processes since the instance should never change.

* Most security testing can be managed during image creation, reducing the need for vulnerability assessment on running workloads since their behavior should be completely known at the time of creation. This doesn't eliminate all security testing for production workloads, but it is a means of offloading large portions of testing.

Immutable does add some requirements:

* You need a consistent image creation process and the automation to support updating deployments. 

* Security testing must be integrated into the image creation and deployment process, including source code tests and, if using virtual machines or standard containers, vulnerability assessments.

* Image configurations need mechanisms to disable logins and restrict services before deploying the images and using them for production virtual machines.

* You may want a process, for some workloads, to enable logins to workloads that aren't actively in the application stack for troubleshooting. This could be a workload pulled from the group but allowed to continue to run in isolation. Alternatively (and often preferred), send sufficiently detailed logs to an external collector so that there is never a need to log in.

* There will be increased complexity to manage the service catalog, since you might create dozens, or even hundreds, of images on any given day.
        
### The impact of cloud on standard workload security controls

Some standard workload controls aren't as viable in cloud workloads (e.g. running antivirus inside some container types). Others aren't necessarily needed or need deep modification to maintain effectiveness in cloud computing:

* You may lose the ability to run software agents for non-VM based workloads, such as those running in "serverless" provider-managed containers.

* "Traditional" agents may impede performance more heavily in cloud. Lightweight agents with lower compute requirements allow better workload distribution and efficient use of resources. Agents not designed for cloud computing may assume underlying compute capacity that isn't aligned with how the cloud deployment is designed. The developers on a given project might assume they are running a fleet of lightweight, single-purpose virtual machines. A security agent not attuned to this environment could significantly increase processing overhead, requiring larger virtual machine types and increasing costs.

* Agents that operate in cloud environments also need to support dynamic cloud workloads and deployment patterns like auto-scaling. They can't rely (on the agent or in the management system) on static IP addressing. While some cloud assets run on static IP addresses, it is far more common for the cloud to dynamically assign IP addresses at run time to enable elasticity. Thus the agent must have the ability to discover the management/control plane and use that to determine what kind of workload it is running on and where.

* The management plane of the agent must itself also operate at the speed of auto-scaling and support elasticity (e.g. be able to keep up with incredibly dynamic IP addressing, such as the same address used by multiple workloads within a single hour). Traditional tools aren't normally designed for this degree of velocity, creating the same issue as we discussed with network security and firewalls.

* Agents shouldn't increase attack surface due to communications/networking or other requirements. While this is always true, there is a greater likelihood of an agent becoming a security risk in cloud for a few reasons:
	* We have a greater ability to run immutable systems, and an agent, like any piece of software, opens up additional attack surface, especially if it ingests configuration changes and signatures that could be used as an attack vector.
	* In cloud we also tend to run fewer different services with a smaller set of networking ports on any given virtual machine (or container), as compared to a physical server. Some agents require opening up additional firewall ports, which increases the network attack surface.
	* This doesn't mean agents always create new security risks, but the benefits need to be balanced before simply assuming the security upside.

* File integrity monitoring can be an effective means of detecting unapproved changes to running immutable instances. Immutable workloads typically require fewer additional security tools, due to their hardened nature. They are locked down more than the usual servers and tend to run a smaller set of services. File integrity monitoring, which tends to be very lightweight, can be a good security control for immutable workloads since you should essentially have zero false positives by their unchanging nature.

* Long-running VMs that still run standard security controls may be more isolated on the network, changing how they are managed. You might experience difficulty in connecting your management tool to a virtual machine running in a private network subnet. While you can technically run the management tool in the same subnet, this could increase costs significantly and be more difficult to manage.

* Cloud workloads running in isolation are typically less resilient than on physical infrastructure, due to the abstraction. Disaster recovery for these is extremely important.

### Changes to workload security monitoring and logging

Security logging/monitoring is more complex in cloud computing:
	
* IP addresses in logs won't necessarily reflect a particular workflow since multiple virtual machines may share the same IP address over a period of time, and some workloads like containers and serverless may not have a recognizable IP address at all. Thus you need to collect some other unique identifiers in the logs to be assured you know what the log entries actual refer to.

* Logs need to be offloaded and collected externally more quickly due to the higher velocity of change in cloud. You can easily lose logs in an auto-scale group if they aren't collected before the cloud controller shuts down an unneeded instance.
	* Logging architectures need to account for cloud storage and networking costs. For example, sending all logs from instances in a public cloud to an on-premises SIEM may be cost prohibitive, due to the extra Internet networking fees.
        
### Changes to vulnerability assessment

Vulnerability assessments in cloud computing need to account for both architectural and contractual limitations:
	
* The cloud owner (public or private) will typically require notification of assessments and place limits on the nature of assessments. This is because they may be unable to distinguish an assessment from a real attack without prior warning.

* Default deny networks further limit the potential effectiveness of an automated network assessment, just as any firewall would. You either need to open up holes to perform the assessment, use an agent on the instance to perform the assessment, or assess knowing that a lot of tests are blocked by the firewall rules.

* Assessments can be run during the image creation process for immutable workloads. Since these aren't in production, and the process is automated, they can run with fewer network restrictions, thus increasing the assessment surface.

* Penetration testing is less affected since it still uses the same scope as an attacker. We cover penetration testing in more detail in Domain 10.

### Cloud storage security

Although part of infrastructure, we cover storage and data security in much more depth in Domain 11.

## Recommendations

* Know the infrastructure security of your provider or platform.
    * In the shared security model, the provider (or whoever maintains the private cloud platform) has the burden of ensuring the underlying physical, abstraction, and orchestration layers of the cloud are secure.
* Network
    * Prefer SDN when available.
    * Use SDN capabilities for multiple virtual networks and multiple cloud accounts/segments to increase network isolation.
        * Separate accounts and virtual networks dramatically limit blast radius compared to traditional data centers.
    * Implement default deny with cloud firewalls.
    * Apply cloud firewalls on a per-workload basis as opposed to a per-network basis.
    * Always restrict traffic between workloads in the same virtual subnet using a cloud firewall (security group) policy whenever possible.
    * Minimize dependency on virtual appliances that restrict elasticity or cause performance bottlenecks.
* Compute/workload
    * Leverage immutable workloads whenever possible.
        * Disable remote access.
        * Integrate security testing into image creation.
        * Alarm with file integrity monitoring.
        * Patch by updating images, not patching running instances.
    * Choose security agents that are cloud-aware and minimize performance impact, if needed.
    * Maintain security controls for long-running workloads, but use tools that are cloud aware.
    * Store logs external to workloads.
    * Understand and comply with cloud provider limitations on vulnerability assessments and penetration testing.