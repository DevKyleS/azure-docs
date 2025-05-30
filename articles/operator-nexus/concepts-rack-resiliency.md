---
title: Operator Nexus rack resiliency
description: Document how rack resiliency works in Operator Nexus Near Edge
ms.topic: article
ms.date: 05/28/2025
author: eak13
ms.author: ekarandjeff
ms.service: azure-operator-nexus
---

# Ensuring control plane resiliency with Operator Nexus Service

The Nexus service is engineered to uphold control plane resiliency across various compute rack configurations.

## Instances with three or more compute racks

Operator Nexus ensures the availability of three active Kubernetes control plane (KCP) nodes in instances with three or more compute racks. For configurations exceeding two compute racks, an extra spare node is also maintained. These nodes are strategically distributed across different racks to guarantee control plane resiliency, when possible.

> [!TIP]
> The Kubernetes control plane is a set of components that manage the state of a Kubernetes cluster, schedule workloads, and respond to cluster events. It includes the API server, etcd storage, scheduler, and controller managers.
>
> The remaining management nodes contain various operators which run the platform software and other components performing support capabilities for monitoring, storage, and networking.

During runtime upgrades, Operator Nexus implements a sequential upgrade of the control plane nodes which preserves resiliency throughout the upgrade process.

Three compute racks:

KCP = Kubernetes Control Plane Node
MGMT = Management Node Pool Node

| Rack 1    | Rack 2 | Rack 3 |
| --------- | ------ | ------ |
| KCP       | KCP    | KCP    |
| KCP-spare | MGMT   | MGMT   |

Four or more compute racks:

| Rack 1 | Rack 2 | Rack 3 | Rack 4    |
| ------ | ------ | ------ | --------- |
| KCP    | KCP    | KCP    | KCP-spare |
| MGMT   | MGMT   | MGMT   | MGMT      |

## Instances with less than three compute racks

Operator Nexus maintains an active control plane node and, if available, a spare control plane instance. For instance, a two-rack configuration has one active Kubernetes Control Plane (KCP) node and one spare node.

Two compute racks:

| Rack 1 | Rack 2    |
| ------ | --------- |
| KCP    | KCP-spare |
| MGMT   | MGMT      |

Single compute rack:

Operator Nexus supports control plane resiliency in single rack configurations by having three management nodes within the rack. For example, a single rack configuration with three management servers provides an equivalent number of active control planes to ensure resiliency within a rack.

| Rack 1 |
| ------ |
| KCP    |
| KCP    |
| KCP    |

## Resiliency implications of lost quorum

In disaster situations when the control plane loses quorum, there are impacts to the Kubernetes API across the instance. This scenario can affect a workload's ability to read and write Custom Resources (CRs) and talk across racks.

## Automated remediation for Kubernetes Control Plane, Management Plane, and Compute nodes

To avoid losing Kubernetes control plane (KCP) quorum, Operator Nexus provides automated remediation when certain server issues are detected. In certain situations, this automated remediation extends to Management Plane & Compute nodes as well.

As a general overview of server resilience, here are the triggers for automated remediation:

- For all servers: if a server fails to provision successfully after four hours, automated remediation occurs.
- For all servers: if a running node is stuck in a read only root filesystem mode for 10 minutes, automated remediation occurs.
- For KCP and Management Plane servers, if a Kubernetes node is in an Unknown state for 30 minutes, automated remediation occurs.

Remediation Process:

- Remediation of a Compute node is one reprovisioning attempt. If the reprovisioning fails, the node is marked `Unhealthy`.
- Remediation of a Management Plane node is to attempt one reboot and then one reprovisioning attempt. If those steps fail, the node is marked `Unhealthy`.
- Remediation of a KCP node is to attempt one reboot. If the reboot fails, the node is marked `Unhealthy` which triggers the immediate provisioning of the spare KCP node.

Ongoing control plane resiliency requires a spare KCP node. When KCP node fails remediation and is marked `Unhealthy`, a deprovisioning of the node occurs, and an exchange with a suitable healthy Management Plane server occurs. This Management Plane server becomes the new spare KCP node. The failed KCP node is updated and labeled as a Management Plane node. Once the label changes, an attempt to provision the newly labeled management plane node occurs. If it fails to provision, the management plane remediation process takes over. If it fails provisioning or doesn't run successfully, the machine's status remains unhealthy and the user must fix. The unhealthy condition surfaces to the Bare Metal Machine's (BMM) `detailedStatus` fields in Azure, and clears through a BMM Replace action.

## Related Links

[Determining Control Plane Role](./reference-near-edge-baremetal-machine-roles.md)

[Troubleshooting failed Control Plane Quorum](./troubleshoot-control-plane-quorum.md)
