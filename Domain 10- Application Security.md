# Domain 10: Application Security

> This domain is in the outline phase. The first draft will be written and opened for feedback after a review period.

> Note: in version 3 of the Guidance this domain contained extensive material that overlapped with 

## Introduction:

* Definition and scope
	* This section of the guidance is for software development and IT teams who want to securely build — and deploy — applications in cloud computing environments, specifically PaaS and IaaS
		* How application security is different in cloud
		* Review of secure software development basics and how those change in the cloud
		* Leveraging cloud capabilities for more secure cloud applications
	* Impact of cloud on application security
		* Opportunities
			* Cloud provider baseline security
			* APIs and automation
			* Hyper-segregation
				* Micro-service architectures
			* Elasticity, orchestration, and immutable infrastructure
			* DevOps
		* Challenges
			* Control and visibility changes
			* Management plane/metastructure security
			* Cloud provider relationship and shared security model
			* Transparency and knowledge of cloud provider capabilities

## Overview

* Introduction to the Security Development Lifecycle and Cloud Computing
	* Defining an SDLC
	* "Meta" phases
		* Phases aren't necessarily linear and are often cyclical with processes that are agile and/or DevOps
		* Secure Design and Development
		* Secure Deployment
		* Secure Operations
	* Impact of cloud computing on SDLC
		* Overview
			* Shared responsibility model
				* Changes in visibility and control
			* Cloud provider capabilities and limitations
			* Management plane/metastructure
			* New and different architectural options
			* Rise and impact of DevOps
		* Secure Design and Development
			* Four phases
				* Define- determine approved architectures or features/tools for the provider, security standards, and other requirements. Define deployment and operations processes due to tight coupling required in cloud computing.
				* Design- focus for security in cloud is on architecture, cloud provider baseline capabilities, cloud provider features, and automating and managing security for deployment and operations. Update threat modeling for cloud
				* Develop- secure development environment. Balance management plane lockdown with required developer flexibility in the metastructure. 
				* Test- Integrate security testing to the deployment process/pipeline. Rely more on automated testing in cloud, and infrastructure more-often in scope due to "infrastructure as code". Feature flagging for security-sensitive capabilities. This bleeds into deployment.
		* Secure Deployment
			* Automation more prominent in cloud
			* Testing often integrated into deployment process and pipelines
				* Impact on code testing
					* Code review/SAST
					* Unit testing, regression testing, security stress testing, fuzzing/DAST and how they change in cloud. 
					* Automated infrastructure/server testing in cloud recommended due to automated deployment pipelines.
				* Impact on vulnerability assessment
				* Impact on penetration testing
			* Secure the deployment pipeline
				* Improvements in auditing/logging and change management
			* Impact of infrastructure as code
			* Security advantages of immutable deployments
		* Secure Operations
			* Restrict the management plane
			* Monitoring/logging for cloud
			* Automating detection and remediation of deviations from policy
				* Must rely on security automation, not manual assessments
			* Ongoing application testing and assessment
				* Vulnerability analysis and penetration testing differences in cloud
			* Change management and integration with deployment pipelines
			* Incident response
				* See the IR domain
* How cloud impacts application design and architectures
	* Key trends and differences:
		* Segregation by default
			* Enhance segregation with virtual networks and accounts
		* Immutable infrastructure
			* E.g. disabling SSH in production
		* Increased use of micro-services and security impact
		* PaaS and "serverless" architectures
			* Potential for dramatically reduced attack surface
		* Role of metastructure/management plane, APIs, and infrastructure and platform automation
		* Software defined security
		* Event driven security
		* Areas for particular focus
			* Identity and Access Management
				* See that Domain
			* Encryption
			* Monitoring/logging
			* Detecting and responding to change
			* See the infrastructure domain for more specifics
* The rise and role of DevOps
	* Definition
	* Deployment pipelines and continuous deployment/delivery
	* Security implications and advantages
		* Standardization
		* Automated testing
		* Supports immutable
		* Improved auditing and change management
	* SecDevOps and Rugged DevOps

## Recommendations

* Understand the security capabilities of your cloud providers. Not merely their baseline, but the various platforms and services.
* Integrate security into the initial design process. Cloud deployments are more-often greenfield, creating new opportunities to engage security early.
* Even if you don't have a formal SDLC, consider moving to continuous deployment and automate security into the deployment pipeline.
* Understand the new architectural options and requirements in the cloud. Update your security policies and standards to support, and don't merely attempt to enforce existing standards on an entirely different computing model.
* Integrate security testing into the deployment process.
* Use software defined security to automate security controls.
* Use event driven security, when available, to automate detection and remediation of security issues.
* Use different cloud environments to control management plane access while still enabling developers.