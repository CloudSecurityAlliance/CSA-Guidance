# Domain 9: Incident Response

## Introduction

Incident Response (IR) is a critical facet of any information security program. Preventive security controls have proven unable to completely eliminate the possibility that critical data could be compromised. Most organizations have some sort of IR plan to govern how they will investigate an attack, but with the distinct differences in both access to forensic data and governance in the cloud, organizations must consider how their IR processes will change. 

This domain seeks to identify those gaps pertinent to IR that are created by the unique characteristics of cloud computing. Security professionals may use this as a reference when developing response plans and conducting other activities during the preparation phase of the IR lifecycle. This domain is organized in accord with the commonly accepted Incident Response Lifecycle as described in the National Institute of Standards and Technology Computer Security Incident Handling Guide (NIST 800-61rev2 08/2012) [1]. Other international standard frameworks for incident response include ISO/IEC 27035 and the ENISA [Strategies for incident response and cyber crisis cooperation](https://www.enisa.europa.eu/publications/strategies-for-incident-response-and-cyber-crisis-cooperation).

After describing the Incident Response Lifecycle, as laid out in NIST 800-61rev2, each subsequent section addresses a phase of the lifecycle and explores the potential considerations for responders as they work in a cloud environment.  

## Overview

### Incident Response Lifecycle

The incident response cycle is defined in the NIST 800-61rev2 document. It includes the following phases and major activities:

* Preparation: "Establishing an incident response capability so that the organization is ready to respond to incidents"
	* Process to handle the incidents.
	* Handler Communications and Facilities.
	* Incident Analysis Hardware and Software.
	* Internal Documentation (Port lists, Asset Lists, Network diagrams, current baselines of network traffic).
	* Identifying training.
	* Evaluating infrastructure by proactive scanning and network monitoring, vulnerability assessments, and performing risk assessments.
	* Subscribing to third-party threat intelligence services.
	
* Detection & Analysis
	* Alerts (Endpoint Protection, Network Security Monitoring, Host Monitoring, Account Creation, Privilege Escalation, other indicators of compromise, SIEM, Security Analytics (baseline and anomaly detection), and user behavior analytics).
	* Validate alerts (reducing false positives) and escalation.
	* Estimate the scope of the Incident.
	* Assign an Incident Manager who will coordinate further actions.
	* Designate a person who will communicate the incident containment and recovery status to Sr. Management.
	* Build a timeline of the attack.
	* Determine the extent of the potential data loss.
	* Notification and coordination activities.
	
* Containment, Eradication & Recovery
	* Containment: Taking systems offline. Considerations for data loss versus service availability. Ensuring systems don't destroy themselves upon detection.
	* Eradication & Recovery: Clean up compromised devices and restore systems to normal operation. Confirm systems are functioning properly, deploy controls to prevent similar incidents.
	* Documenting the incident and gathering evidence (chain of custody).
	
* Post-mortem
	* What could have been done better? Could the attack have been detected sooner? What additional data would have been helpful to isolate the attack faster? Does the IR process need to change? If so, how?
	
### How the cloud impacts I/R

Each of the phases of the lifecycle is affected to different degrees by a cloud deployment. Some of these are similar to any incident response in an outsourced environment where you need to coordinate with a third party. Other differences are more specific to the abstracted and automated nature of cloud. 

#### Preparation
	
When preparing for cloud incident response, here are some major considerations:

* *SLAs and Governance:* Any incident using a public cloud or hosted provider requires an understanding of service level agreements (SLAs) and likely coordination with the cloud provider. Keep in mind that, depending on your relationship with the provider, you may not have direct points of contact and might be limited to whatever is offered through standard support. A custom private cloud in a third-party datacenter will have a very different relationship than signing up through a website and clicking through a license agreement for a new SaaS application.

Key questions include: What does your organization do? What is the cloud service provider (CSP) responsible for? Who are the points of contact? What are the response time expectations? What are the escalation procedures? Do you have out-of-band communication procedures (in case networks are impacted)? How do the hand-offs work? What data are you going to have access to?

Be sure to test the process with the CSP if possible. Validate that escalations and roles/responsibilities are clear. Ensure the CSP has contacts to notify you of incidents they detect, and that it's integrated into your process. For click-through services, notifications will likely be sent to your registration email address; these should be controlled by the enterprise and monitored continuously. Ensure that you have contacts, including via out-of-band methods, for your CSP and that you test them.
				
* *IaaS/PaaS vs. SaaS:* In a multi-tenant environment, how can data specific to your cloud be provided for investigation? For each major service you should understand and document what data and logs will be available in an incident. Don't assume you can contact a provider after the fact and collect data that isn't normally available.
			
* *"Cloud jump kit:"* These are the tools needed to investigate in a remote location (as with cloud-based resources). For example, do you have tools to collect logs and metadata from the cloud platform? Do you have the ability to interpret the information? How do you obtain images of running virtual machines and what kind of data do you have access to: disk storage or volatile memory?
			
* *Architect the cloud environment for faster detection, investigation, and response (containment and recoverability).* This means ensuring you have the proper configuration and architecture to support incident response:
				
	* Enable instrumentation, such as cloud API logs, and ensure that they feed to a secure location that's available to investigators in case of an incident.
	* Utilize isolation, to ensure that attacks cannot spread and compromise the entire application.
	* Use immutable servers when possible. If an issue is detected, move workloads from compromised device onto a new instance in a known-good state. Employ a greater focus on file integrity monitoring and configuration management.
	* Implement application stack maps to understand where data is going to reside, in order to factor in geographic differences in monitoring and data capture.
	* It can be very helpful to perform threat modeling and tabletop exercises to determine the most effective means of containment for different types of attacks on different components in the cloud stack.
	* This should include differences between responses for IaaS/PaaS/SaaS.

#### Detection and Analysis

Detection and analysis in a cloud environment may look nearly the same (for IaaS) and quite different (for SaaS). In all cases, the monitoring scope must cover the cloud's management plane, not merely the deployed assets.

You may be able to leverage in-cloud monitoring and alerts that can kick off an automated I/R workflows in order to speed up the response process. Some cloud providers offer these features for their platforms, and there are are also some third-party monitoring options available. These may not be security-specific: many cloud platforms (IaaS and possibly PaaS) expose a variety of real-time and near-real-time monitoring metrics for performance and operational reasons. But security may be able to also leverage these for security needs.

Cloud platforms also offer a variety of logs, which can sometimes be integrated into existing security operations/monitoring. These could range from operational logs to full logging of all API calls or management activity. Keep in mind that they are not available on all providers; you tend to see them more with IaaS and PaaS than SaaS. When log feeds aren't available you may be able to use the cloud console as a means to identify environment/configuration changes. 
	
*Data sources* for cloud incidents can be quite different from those used in incident response for traditional computing. There is significant overlap, such as system logs, but there are differences in terms of how data can be collected and in terms of new sources, such as feeds from the cloud management plane.

As mentioned, cloud platform logs may be an option, but they are not universally available. Ideally they should show all management-plane activity. It's important to understand what is logged and the gaps that could affect incident analysis. Is all management activity recorded? Do they include automated system activities (like auto scaling) or cloud provider management activities? Providers may have other logs available in a serious incident that are not normally available to customers.

One challenge in collecting information may be limited network visibility. Network logs from a cloud provider will tend to be flow records, but not full packet capture. 

Where there are gaps you can sometimes instrument the technology stack with your own logging. This can work within instances, containers, and application code in order to gain telemetry important for the investigation. Pay particular attention to PaaS and serverless application architectures; you will likely need to add custom application level logging.

External threat intelligence may also be useful, as it is with on-premises incident response, in order to help identify indicators of compromise and get adversary information.

Be aware that there are potential challenges when the information that is provided by a CSP faces chain of custody questions. There are no reliable precedents established at this point. 

*Forensics and investigative support* will also need to adapt, beyond understanding changes to data sources.
				
Always factor in what the CSP can provide and whether it meets chain of custody requirements. Not every incident will result in legal action, but it's important to work with your legal team to understand the lines and where you could end up having chain of custody issues.
				
There is a greater need to automate many of the forensic/investigation processes in cloud environments, because of its dynamic and higher-velocity nature. For example, evidence could be lost due to a normal auto-scaling activity or if an administrator decides to terminate a virtual machine involved in an investigation. Some examples of tasks you can automate include:

* Snapshotting the storage of the virtual machine.
* Capturing any metadata at the time of alert, so that the analysis can happen based on what the infrastructure looked like at that time.
* If your provider supports it, "pausing" the virtual machine, which will save the volatile memory state.

You can also leverage the capabilities of the cloud platform to determine the extent of the potential compromise: 

* Analyze network flows to check if network isolation held up. You can also use API calls to snaphot the network and the virtual firewall rules state, which could give you an accurate picture of the entire stack at the time of the incident.
* Examine configuration data to check if other similar instances were potentially exposed in the same attack. 
* Review data access logs (for cloud-based storage, if available) and management plane logs to see if the incident affected or spanned into the cloud platform.
* Serverless and PaaS-based architectures will require additional correlation across the cloud platform and any self-generated application logs.
	
#### Containment, Eradication and Recovery

Always start by ensuring the cloud management plane/metastructure is free of an attacker. This will often involve invoking break-class procedures to access the root or master credentials for the cloud account, in order to ensure that attacker activity isn't being masked or hidden from lower-level administrator accounts. Remember: You can't contain an attack if the attacker is still in the management plane. Attacks on cloud assets, such as virtual machines, may sometimes reveal management plane credentials that are then used to bridge into a wider, more serious attack.

The cloud often provides a lot more flexibility in this phase of the response, especially for IaaS. Software-defined infrastructure allows you to quickly rebuild from scratch in a clean environment, and, for more-isolated attacks, inherent cloud characteristics—such as auto-scale groups, API calls for changing virtual network or machine configurations, or snapshots—can speed quarantine, eradication, and recovery processes. For example, on many platforms you can instantly quarantine virtual machines by moving the instance out of the auto-scale group, isolating it with virtual firewalls, and replacing it.

This also means there's no need to immediate "eradicate" the attacker before you identify their exploit mechanisms and the scope of the breach, since the new infrastructure/instances are clean; instead, you can simply isolate them. However, you still need to ensure the exploit path is closed and can't be used to infiltrate other production assets. If there is concern that the management plane is breached, be sure to confirm that the templates or configurations for new infrastructure/applications have not been compromised. 

That said, these capabilities are not always universal: with SaaS and some PaaS you may be very limited and will thus need to rely more on the cloud provider.
	
#### Post-mortem

As with any attack, work with the internal response team and provider to figure what worked, what didn't and pinpoint any areas for improvement. Pay particular attention to the limitations in the data collected and figure out how to address the issues moving forward.

It is hard to change SLAs, but if the agreed-upon response time, data, or other support wasn't sufficient, go back and try to renegotiate.
	
## Recommendations

* SLAs and setting expectations around what the customer does versus what the provider does are the most important aspect of incident response for cloud-based resources. Clear communication of roles/responsibilities and practicing the response and hand-offs are critical.
* Cloud customers must set up proper communication paths with the provider that can be utilized in the event of an incident. Existing open standards can facilitate incident communication.  
* Cloud customers must understand the content and format of data that the cloud provider will supply for analysis purposes and evaluate whether the available forensics data satisfies legal chain of custody requirements. 
* Cloud customers should also embrace continuous and serverless monitoring of cloud-based resources to detect potential issues earlier than in traditional data centers.
	* Data sources should be stored or copied into locations that maintain availability during incidents.
	* If needed and possible, they should also be handled to maintain a proper chain of custody.
* Cloud-based applications should leverage automation and orchestration to streamline and accelerate the response, including containment and recovery. 
* For each cloud-service provider used, the approach to detecting and handling incidents involving the resources hosted at that provider must be planned and described in the enterprise incident response plan.
* The SLA with each cloud-service provider must guarantee the support for incident handling required for the effective execution of the enterprise incident response plan. This must cover each stage of the incident handling process: detection, analysis, containment, eradication, and recovery.
* Testing will be conducted at least annually or whenever there are significant changes to the application architecture. Customers should seek to integrate their testing procedures with that of their provider (and other partners) to the greatest extent possible.