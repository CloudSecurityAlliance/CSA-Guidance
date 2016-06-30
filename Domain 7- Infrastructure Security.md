
# Domain 7: Infrastructure Security  
  Core cloud infrastructure security, including networking, workload security, and hybrid cloud considerations. This domain also includes security fundamentals for private clouds.


## Introduction

* Infrastructure security is the foundation for operating securely in the cloud.
* It includes the lowest layers of security, from physical facilities through the customer's configuration and implementation of infrastructure components.
    * This includes compute (workload), networking, and storage security.
    * Storage security is covered in Domain 11 this domain focuses on network and workload security, including hybrid cloud considerations.
* Fundamental data center security is beyond the scope of the CSA Guidance. Cloud providers and private cloud users should refer to existing industry standards.

## Overview

* Defining infrastructure
    * Two layers
        * The fundamental resources pooled together to create a cloud. E.g. Computers, networks, and storage.
        * The virtual/abstracted infrastructure managed by a cloud consumer. The compute, network, and storage assets they use from the resource pools,
    * This domain primarily focuses on infrastructure security for the cloud consumer. Infrastructure security for cloud providers, including those who manage private clouds, is well aligned with existing security standards for data centers.
* Cloud network security
    * All clouds utilize virtual networks.
        * This is the only way to create abstraction from a physical network.
    * Two major categories
        * VLAN based
            * Leverage existing network technology implemented in most network hardware.
            * Designed for use in single-tenant networks (enterprise data centers) to separate different business units, functions, etc. (like guest networks).
            * Uses a fixed pool of non-overlapping IP addresses and other network identifiers.
        * Software Defined Networking (SDN)
            * A more-complete abstraction layer on top of networking hardware.
            * Multiple implementations, including standards based and proprietary.
            * Depending on the implementation, can offer much higher flexibility and isolation. For example, multiple, segregated overlapping IP ranges for a virtual network on top of the same physical network.
            * Typically offers software definition of arbitrary IP ranges, allowing customers to better extend their existing networks into the cloud.
            * May look like a regular network on the surface to a cloud consumer, but being a more-complete abstraction will function very differently under the surface.
            * Very commonly uses packet encapsulation so virtual machines and other "standard" assets don't need any changes to their network stack.
    * How security changes
        * Lack of physical network changes common network practices.
            * Sniffing either not possible or should be disabled by default.
                * Customer security tools thus need to rely on an in-line virtual appliance, or a software agent installed in instances.
            * Physical appliances can't be inserted (except by the cloud provider). They must be replaced by virtual appliances, if the cloud network supports the necessary routing.
                * Virtual appliances thus become bottlenecks, since they cannot fail open, and must intercept all traffic.
                * Virtual appliances may take significant resources and increase costs to meet network performance requirements.
                * Virtual appliances should support auto scaling to match the elasticity of the resources they protect.
            * Cloud application components tend to be more distributed to improve resiliency, and due to auto scaling virtual servers may have shorter lives and be more prolific. This changes how security policies need to be designed.
                * This induces a very high rate of change that security tools must be able to manage (e.g. Servers with a lifespan of less than an hour).
                * IP addresses will change far more quickly than on a traditional network, which security tools must account for.
                * Assets are less likely to exist at static IP addresses.
                * Different assets may share the same IP address within a short period of time.
                * Assets within a single application tier will often be located on multiple subnets for resiliency, further complicating IP-based security policies.
                * Fewer services per server improves your ability to define restrictive firewall rules.
        * Software defined networks enable new types of security controls
            * Isolation is easier. Possible to build out as many isolated networks as you need without constraints of physical hardware.
            * SDN firewalls (e.g, security groups) can apply to assets based on more flexible criteria than hardware based firewalls since they aren't limited based on physical topology,
                * SDN firewalls are typically policy sets that define ingress and egress rules that can apply to single assets or groups of assets regardless of network location (within a given virtual network).
                * Combined with the cloud platform's orchestration layer, this enables very dynamic and granular combinations and policies with less management overhead than the equivalent using a traditional hardware or host-based approaches.
                * Default deny is often the starting point, and you are required to open connections from there, which is the opposite of most physical network firewalls.
                * Think of it as the granularity of a host firewall with better manageability than a network appliance.
                * Can also potentially apply on other criteria, such as tags.
                * Note that while the potential is there, the actual capabilities depend on the platform.
            * Many network attacks are eliminated by default (depending on your platforms) such as ARP spoofing and other lower level exploits, beyond merely eliminating sniffing.
            * Potential is there for encryption by default as packets are encapsulated.
            * As with security groups, other routing and network design can by dynamic and tied to the cloud's orchestration layer.
            * Additional security functions can be added natively.
        * For cloud providers or private clouds, additional efforts required.
            * Must maintain the core security of the physical/traditional network the platform is built on.
            * Must do so with arbitrary communications and multiple tenants.
            * Absolutely critical to maintain segregation and isolation for the multi tenant environment.
            * Additional overhead to properly enable, configure, and maintain the SDN security controls.
            * Must also expose security controls to the cloud consumers.
            * Will need to implement perimeter security that protects the environment, but minimizes impact on customer workloads.
    * Hybrid cloud considerations
        * Ideally the hybrid cloud will support arbitrary network addressing to help seamlessly extend the cloud consumer's network.
        * The hybrid connection may reduce the security of the cloud network if the private network isn't at an equivalent security level.
        * A hybrid connection shouldn't effectively flatten the security of both networks.
            * Should be enforced via routing, access controls, and even firewalls or additional network security tools.
        * It is typically preferable to minimize hybrid connections. They are often still necessary, but don't assume they are needed.
            * They increase routing complexity.
            * They can reduce the ability to run multiple cloud networks with overlapping IP ranges.
            * They complicate security on both sides due to the need to harmonize security controls.
* Cloud compute and workload security
    * A workload is a unit of processing, which can be in a virtual machine, a container, or other abstraction.
    * Workloads always run, somewhere, on a processor and consume memory.
    * How cloud changes workload security
        * Multiple workloads will nearly always share the same underlying physical processor and memory.
            * Multiple tenants will likely share the same physical compute node.
                * Dedicated/private tenancy is possible, but typically at a higher cost (for public cloud as a consumer, and private cloud due to less efficient use of resources).
            * The consumer rarely gets to control where a workload physically runs.
            * There are multiple abstraction types, each with differing degrees of segregation and isolation.
                * Virtual machines
                    * Most well known, and can tie into underlying hardware capabilities to reinforce isolation.
                    * Potentially open to certain memory attacks, but this is increasingly difficult.
                * Containers
                    * Multiple containers can run on the same virtual machine or be implemented without the use of VMs.
                    * Newer, with differing isolation capabilities that are very platform dependent.
                * Platform-based workloads (e.g. logic/procedures running on a shared database platform)
                    * Isolation security totally the responsibility of the platform provider.
        * Immutable workloads enable security.
            * Autoscaling and containers, by nature, work best you run instances launched dynamically based on an image, and those instances can be shut down when no longer needed for capacity without breaking an application stack.
            * Thus you no longer patch or make other changes to a running workload, since that wouldn't change the image, and thus new instances would be out of sync.
            * Instead you update the underlying image, then rotate the new instances by shutting down the old ones and running the new ones.
            * This enables significant security benefits.
                * You no longer patch running systems and worry about dependencies, broken patch processes, etc. you replace them with a new gold master.
                * You can, and should, disable remote logins to running workloads (if logins are even an option). This is an operational requirement to prevent changes that aren't consistent across the stack, which also has significant security benefit.
                * It is much faster to roll out updated versions since applications must be designed to handle individual nodes going down (remember, fundamental to any auto scaling).
                * It is easier to disable services and whitelist applications/processes.
                * Most security testing can be managed during image creation, reducing the need for vulnerability assessment on running workloads since their behavior should be completely known.
            * This does add requirements.
                * Need a consistent image creation process.
                * Must integrate security testing into the process, including source code tests and, if using virtual machines or standard containers, vulnerability assessment.
                * Need mechanisms to disable logins and restrict services.
                    * May want a process, for some workloads, to enable logins to workloads that aren't actively in the application stack for troubleshooting. This could be a workload pulled from the group but allowed to continue to run in isolation.
                    * Alternatively (and often preferred) send detailed-enough logs to an external collector so there is never a need to log in.
                * Increased complexity to manage service catalog.
        * Some standard workload controls aren't possible (e.g. running antivirus inside some container types). Others aren't necessarily needed.
            * May lose ability to run software agents for non-VM based workloads.
            * Agents may impede performance requirements.
                * Lightweight agents with lower compute requirements allow better workload distribution and efficient use of resources.
            * Agents that do run need to support dynamic cloud workloads.
                * Must not rely on static IP addressing.
                * Must have ability to discover management/control plane.
                * Management plane must operate at speed of auto scaling and support elasticity (e.g. Be able to keep up with incredibly dynamic IP addressing, such as the same address used by multiple workloads within a single hour).
                * Agents shouldn't increase attack surface due to communications/networking or other requirements,
            * Immutable workloads typically require fewer additional security tools due to their hardened nature.
        * Long running VMs still need standard security controls, but have different management requirements.
            * May be more isolated on the network, changing how they are managed.
            * Cloud workloads running in isolation are typically less resilient than on physical infrastructure due to the abstraction. Disaster recovery is even more important.
        * Security logging/monitoring is more complex.
            * IP addresses won't reflect a particular workflow
            * Logs need to be offloaded and collected externally more quickly.
            * Logging architectures need to account for cloud storage and networking costs. For example, sending all logs from instances in a public cloud to an on premise SIEM may be cost prohibitive.
        * Vulnerability assessment changes.
            * The cloud owner (public or private) will typically require notification of assessments and place limits on the nature of assessments.
            * This is because they may be unable to distinguish an assessment from a real attack without prior warning.
            * Default deny networks further limit potential effectiveness of an assessment, just like any firewall.
            * Assessments can be run during the image creation process for immutable workloads.
* Cloud storage security
    * Covered in more depth in Domain 11
* SaaS and PaaS
    * The SaaS or PaaS provider is completely responsible for infrastructure security.

## Recommendations

* Know the infrastructure security of your provider or platform.
    * In the shared security model, the provider (or whoever maintains the private cloud platform) has the burden to ensure the underlying physical, abstraction, and orchestration layers of the cloud are secure.
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
        * When immutable, patch by updating images, not deploying into running instances.
    * Choose security agents that are cloud-aware and minimize performance impact, if needed.
    * Maintain security controls for long-running workloads, but use tools that are cloud aware.
    * Store logs external to workloads.
    * Understand and comply with cloud provider limitations on vulnerability assessments and penetration testing.
