
![Deepfence Logo](images/Deepfence-Logo_Black.jpg)

# Deepfence Runtime Threat Mapper
The Deepfence Runtime Threat Mapper is a subset of the Deepfence cloud native workload protection platform, released as a community edition. This community edition empowers the users with following features:

1. ***Visualization***: Visualize kubernetes clusters, virtual machines, containers and images, running processes, and network connections in near real time.

2. ***Runtime Vulnerability Management***: Perform vulnerability scans on running containers & hosts as well as container images. 

3. ***Container Registry Scanning***: Check for vulnerabilities in images stored on Docker hub, Docker private registry, AWS ECR, Azure Container Registry, Harbor, Google Container Registry, GitLab and JFrog container registries. 

4. ***CI/CD Scanning***: Scan images as part of existing [CI/CD Pipelines] like CircleCI, Jenkins & GitLab.

5. ***Integrations with SIEM, Notification Channels & Ticketing***: Ready to use integrations with Slack, PagerDuty, HTTP endpoint, Jira, Splunk, ELK, Sumo Logic and Amazon S3.

[Harbor]: https://goharbor.io/
[CI/CD Pipelines]: ci-cd-Integrations

# Live Demo

https://community.deepfence.show/

Username: `community@deepfence.io`

Password: `mzHAmWa!89zRD$KMIZ@ot4SiO` 

# Contents

* [Architecture](#architecture)
* [Features](#feature-availability)
* [Getting Started](#getting-started)
  * [Deepfence management Console](#deepfence-management-console)
    * [Pre-Requisites](#pre-requisites)
    * [Installation](#installation)
  * [Deepfence Agent](#deepfence-agent)
    * [Pre-Requisites](#pre-requisities)
    * [Installation](#installation)
      * [Deepfence Agent on Standalone VM or Host](#deepfence-agent-on-standalone-vm-or-host)
      * [Deepfence Agent on Amazon ECS](#deepfence-agent-on-amazon-ecs)
      * [Deepfence Agent on Google GKE](#deepfence-agent-on-google-gke)
      * [Deepfence Agent on Self-managed/On-premise Kubernetes](#deepfence-agent-on-self-managedon-premise-kubernetes)
* [How do I use Deepfence?](#how-do-i-use-deepfence)
    * [Register a User](#register-a-user)
    * [Use case - Visualization](#use-case---visualization)
    * [Use Case - Runtime Vulnerability Management](#use-case---runtime-vulnerability-management)
    * [Use Case - Registry Scanning](#use-case---registry-scanning)
    * [Use Case - CI/CD Integration](#use-case---cicd-integration)
    * [Use Case - Notification Channel and SIEM Integration](#use-case---notification-channel-and-siem-integration)
    * [API Support](#api-support)
* [Security](#security)
* [Support](#support)


# Architecture

A pictorial depiction of the Deepfence Architecture is below


![Deepfence Architecture](images/DF_Architecture.png)


# Feature Availability

Features      | Runtime Threat mapper (Community Edition) | Workload Protection Platform (Enterprise Edition)
------------- | ----------------- | -------------  
Discover & Visualize Running Pods, Containers and Hosts   | :heavy_check_mark: (upto 100 hosts) | :heavy_check_mark: (unlimited)
Runtime Vulnerability Management for hosts/VMs | :heavy_check_mark: (upto 100 hosts) | :heavy_check_mark: (unlimited)
Runtime Vulnerability Management for containers | :heavy_check_mark: (unlimited) | :heavy_check_mark: (unlimited)
Container Registry Scanning  | :heavy_check_mark:  | :heavy_check_mark:
CI/CD Integration  | :heavy_check_mark:  | :heavy_check_mark:
Multiple Clusters  | :heavy_check_mark:  | :heavy_check_mark:
Integrations with SIEMs, Slack and more | :heavy_check_mark:  | :heavy_check_mark:
Compliance Automation  | :x:  | :heavy_check_mark:
Deep Packet Inspection of Encrypted & Plain Traffic  | :x:  | :heavy_check_mark:
API Inspection  | :x:  | :heavy_check_mark:
Runtime Integrity Monitoring  | :x:  | :heavy_check_mark:
Network Connection & Resource Access Anomaly Detection  | :x:  | :heavy_check_mark:
Workload Firewall for Containers, Pods and Hosts  | :x:  | :heavy_check_mark:
Quarantine & Network Protection Policies | :x:  | :heavy_check_mark:
Alert Correlation  | :x:  | :heavy_check_mark:
Serverless Protection  | :x:  | :heavy_check_mark:
Windows Protection  | :x:  | :heavy_check_mark:
Highly Available & Multi-node Deployment  | :x:  | :heavy_check_mark:
Multi-tenancy & User Management  | :x:  | :heavy_check_mark:
Enterprise Support  | :x:  | :heavy_check_mark:


# Getting Started

The Deepfence Management Console is first installed on a separate system. The Deepfence agents are then installed onto bare-metal servers, Virtual Machines, or Kubernetes clusters where the application workloads are deployed, so that the host systems, or the application workloads, can be scanned for vulnerabilities.

A pictorial depiction of the Deepfence security platform is as follows: 

<!---
   [Comment]: # (![Communication Diagram](images/agent-master-communicate.jpg))
-->   
   ![Deployment Diagram](images/DF_DeploymentDiagram_New.png)
   
## Deepfence Management Console

### Pre-Requisites

Feature       | Requirements
------------- | ----------------- 
CPU: No of cores | 8
RAM | 16 GB
Disk space | At-least 120 GB
Port range to be opened for receiving data from Deepfence agents | 8000 - 8010
Port to be opened for web browsers to be able to communicate with the Management console to view the UI | 443
[Docker] binaries | *At-least version 18.03*
[Docker-compose] binary | *Version 1.20.1*

[Docker]: https://docs.docker.com/install/
[docker-compose]: https://github.com/docker/compose/releases/tag/1.20.1

### Installation

Installing the Management Console is as easy as:

1. Download the file [docker-compose.yml](files/docker-compose.yml) to the desired system.
2. Execute the following command
```shell script
docker-compose -f docker-compose.yml up -d
```

This is the minimal installation required to quickly get started on scanning various container images. The necessary images may now be downloaded onto this Management Console and scanned for vulnerabilities.

## Deepfence Agent

In order to check a host for vulnerabilities, or if docker images or containers that have to be checked for vulnerabilities are saved on different hosts, then the Deepfence agent needs to be installed on those hosts.

### Pre-Requisities

Feature       | Requirements
------------- | ----------------- 
CPU: No of cores | 2
RAM | 4 GB
Disk space | At-least 30 GB
Connectivity | The host on which the Deepfence Agent is to be installed, is able to communicate with the Management Console on port range 8000-8010. 
Linux kernel version | >= 4.4
[Docker] binaries | *At-least version 18.03*
Deepfence Management Console | Installed on a host with IP Address a.b.c.d

### Installation

Installation procedure for the Deepfence agent depends on the environment that is being used. Instructions for installing Deepfence agent on some of the common platforms are given in detail below:

#### Deepfence Agent on Standalone VM or Host

Installing the Deepfence Agent is now as easy as:

1. In the following docker run command, replace x.x.x.x with the IP address of the Management Console
```
docker run -dit --cpus=".2" --name=deepfence-agent --restart on-failure --pid=host --net=host --privileged=true -v /var/log/fenced -v /var/run/docker.sock:/var/run/docker.sock -v /:/fenced/mnt/host/:ro  -v /sys/kernel/debug:/sys/kernel/debug:rw -e DF_BACKEND_IP="x.x.x.x" deepfenceio/deepfence_agent_ce:latest
```

#### Deepfence Agent on Amazon ECS

For detailed instructions to deploy agents on Amazon ECS, please refer to our [Amazon ECS](https://github.com/deepfence/ThreatMapper/wiki/Amazon-ECS-Deployment) wiki page.

#### Deepfence Agent on Google GKE

For detailed instructions to deploy agents on Google GKE, please refer to our [Google GKE](https://github.com/deepfence/ThreatMapper/wiki/Google-Kubernetes-Engine-Deployment) wiki page.

#### Deepfence Agent on Self-managed/On-premise Kubernetes

For detailed instructions to deploy agents on Google GKE, please refer to our [Self-managed/On-premise Kubernetes](https://github.com/deepfence/ThreatMapper/wiki/Kubernetes-(Self-managed-or-on-premise)) wiki page.

## How do I use Deepfence?

Now that the Deepfence Security Platform has been successfully installed, here
are the steps to begin --


### Register a User

The first step is to register a user with the Management Console.

1. If the Management Console has been installed on a system with IP address
   x.x.x.x, fire up a browser (Chromium (Chrome, Safari) is the supported browser for now), and
   navigate to https://x.x.x.x/

![Registration](images/DF_Registration.png)

**After registration, it can take anywhere between 30-60 minutes for initial vulnerability data to be populated. The download status of the vulnerability data is reflected on the notification panel.**

### Use case - Visualization

You can visualize the entire topology of your running VMs, hosts, containers etc. from the topology tab. You can click on individual nodes to initiate various tasks like vulnerability scanning.

![Visualization](images/DF_Visualization.png)

### Use Case - Runtime Vulnerability Management

From the topology view, runtime vulnerability scanning for running containers & hosts can be initiated using the console dashboard, or by using the APIs. Here is snapshot of runtime vulnerability scan on a host node.

![Vulnerability Scanning](images/DF_Vulnerability1.png)

The vulnerabilities and security advisories for each node, can be viewed by navigating to Vulnerabilities menu as follows:

![Vulnerability Scanning](images/DF_Vulnerability2.png)

Clicking on an item in the above image gives a detailed view as in the image below:

![Vulnerability Scanning](images/DF_Vulnerability3.png)

Optionally, users can tag a subset of nodes using user defined tags and scan a subset of nodes as explained in our [user tags wiki page](https://github.com/deepfence/ThreatMapper/wiki/Vulnerability-Scanning-Using-Tags).

### Use Case - Registry Scanning

You can scan for vulnerabilities in images stored in Docker private registry, AWS ECR, Azure Container Registry and Harbor from the registry scanning dashboard. First, you will need to click the "Add registry" button and add the credentials to populate available images. After that you can select the images to scan and click the scan button as shown the image below: 

![Registry Scanning](images/DF_RegistryScanning.png)

### Use Case - CI/CD Integration

For CircleCI integration, refer to our [CircleCI](https://github.com/deepfence/ThreatMapper/wiki/CircleCI-Integration) wiki and for Jenkins integration, refer to our [Jenkins](https://github.com/deepfence/ThreatMapper/wiki/Jenkins-Integration) wiki page for detailed instructions.

### Use Case - Notification Channel and SIEM Integration

Deepfence logs and scanning reports can be routed to various SIEMs and notifications channels by navigating to Notifications screen.

![NotificationIntegrations](images/DF_Notification.png)

For detailed instructions on integrations with slack refer to our [slack wiki page](https://github.com/deepfence/ThreatMapper/wiki/Slack-Integration)

For detailed instructions on integrations with sumo logic refer to our [sumo logic wiki page](https://github.com/deepfence/ThreatMapper/wiki/Sumo-Logic-Integration) 

For detailed instructions on integrations with PagerDuty refer to our [PagerDuty wiki page](https://github.com/deepfence/ThreatMapper/wiki/PagerDuty-Integration)

### API Support

Deepfence provides a suite of powerful API’s to control the features, and to extract various reports from the platform. The documentation of the API’s are available [here](https://github.com/deepfence/deepfence_runtime_api "Deepfence API"), along with sample programs for Python and GO languages.

### Security

Users are strongly advised to control access to the Deepfence Management Console, so that it is only accessible on port range 8000-8010 from those systems that have installed the Deepfence agent. Further, we recommend to open port 443 on the Deepfence Management Console only for those systems that need to use a Web Browser to access the Management Console.

We periodically scan our own images for vulnerabilities and pulling latest images should always give you most secure Deepfence images. In case you want to report a vulnerability in the Deepfence images, please reach out to us by email -- (community at deepfence dot io).


### Support

Please file Github issues as needed.  
