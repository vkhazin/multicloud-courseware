# Multi-Cloud Workloads Courseware

## Overview

There is plenty of courses and certifications specific to a cloud provider  
Understandably, every cloud provider pursues its own interests of locking customers in  
The common thought in an organization: we don't want to be locked-in with a cloud provider  
There is a number of possible solutions for maintaining cloud interoperability:

* Run all workloads on virtual machines without a dependency on cloud-specific services
* Run all workloads in containers leveraging [Kubernetes](https://kubernetes.io/)
* Rely on a 3rd party Platform-as-a-Service e.g. [Pivotal Cloud Foundry](https://pivotal.io/platform)
* Introduce an adapter pattern into your software to abstract business logic from cloud-specific code

The purpose of this course is to analyze the pros and cons of the different approaches  
And to experience the different approaches with pragmatic hands-on labs and group discussions

## Audience

* Software architects evaluating the strategies for an inter-cloud and multi-cloud deployment models
* Software developers writing code that needs to run on different or multiple cloud providers
* DevOps engineers that will be implementing inter-cloud ci/cd

## Course's Objectives

* Review the common design patterns deployment models: IaaS, PaaS, KaaS, DBaaS, and Serverless
* Highlight best design practices for the cloud-native applications: [the twelve factors app](https://12factor.net/)
* Automate deployment of IaaS to a couple of public cloud providers: Aws, GCP or Azure leveraging [terraform](https://www.terraform.io/)
* Deploy an end-point application to a virtual machine leveraging an automated deployment process
* Contrast a cloud-specific logging and monitoring facilities to a centralized logging/monitoring services leveraging 3rd party service
* Demonstrate the process of authoring a Dockerfile to run the end-point application in a container
* Make use of Kubernetes as a service to deploy and monitor the created image
* Get familiar with Pivotal Cloud Foundry PaaS concepts for multi-cloud deployment
* Deploy end-point to Pivotal PaaS and access logs/monitoring tools
* Convert the end-point to a serverless function and deploy to a couple of cloud providers: AWS, GCP, or Azure
* Analyze serverless logs and monitoring
* Discuss the data persistency options compatible with inter-cloud deployments
* Review common security and regulatory concerns

## Prerequisites

* Previous experience with public cloud providers: Aws, GCP, or Azure
* Familiarity with Micro-services concepts
* Recent hands-on experience with terminal/command line
* Database and data-modeling awareness
* The notion of security and compliance concerns

## Outline

### Introduction

* Public cloud providers: concepts, history, setup, and basic operations
* Terminology: IaaS, PaaS, CaaS, Kubernetes, DBaaS, and Serverless
* Distributed and decentralized workloads
* The twelve factors app
* Micro-services principals

### Infrastructure as a Code

* Objectives, Motivation and Principals
* Tools and approaches
* Introduction to Terraform
* IaaS deployment using Terraform

### Logging and Monitoring

* Cloud Provider specific logging and monitoring
* The motivation for a 3rd Party Service Provider
* Selecting a service provider
* Integrating IaaS with a service provider
* Comparison of Cloud Specific and 3rd Party Services

### Docker and Kubernetes

* Motivation and Enthusiasm
* Compare Containers to Virtual Machines
* Author a Dockerfile
* Build Image and Run Container
* Kubernetes as Container Platform
* Author and deploy a pod

### Kubernetes Logging and Monitoring

* Kubernetes native tools
* Logging and Monitoring Practices
* 3rd Party Services
* Comparison of Platform Specific and 3rd Party Services

### Multi-Cloud PaaS

* Motivation and Premise
* PaaS vs. IaaS vs. KaaS
* PaaS Design Considerations
* Deployment automation

### PaaS Logging and Monitoring

* Platform native tools
* Logging and Monitoring Practices
* 3rd Party Services
* Comparison of Platform Specific and 3rd Party Services

### Serverless

* Best of Containers and PaaS or a hype
* Architectural principals
* Coding practices
* Convert end-point to a function
* Cloud specific platforms
* Inter-cloud approaches

### Serverless Logging and Monitoring

* Platform native tools
* Logging and Monitoring Practices
* 3rd Party Services
* Comparison of Cloud Specific and 3rd Party Services

### DBaaS

* Advantages and Liabilities of DBaaS
* High Availability
* Disaster Recovery
* Inter-cloud practices

### Security and Compliance

* Common concerns
* Multi-cloud practices
* Cloud Provider Compliance
* Inter-cloud Authentication
* Audit and Logging

### Duration: 2 days

### Future Ideas:

* Microsoft Azure Arc



