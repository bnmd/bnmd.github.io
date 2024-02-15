---
layout: post
title: "📜 VXRAIL INFRASTRUCTURE Daily Health Report"
categories: IT Vmware
tags: [vmware, vxrail, report]
author: thu4nvd
---

## Objectif

Check the overall health of VXRAIL cluster.

## Prerequisites

- Access to vCenter

## Procedure & Validation

- Connect to vSphere using the S1 account
## > Check the state of the hosts

- Choose the **Datacenter > Hosts & Cluster > Hosts**:   
  Check the value of status is Normal
- Validation: All the hosts must be in « Normal » state  
  (In the case of a « Warning » state, there should be no incident reported, except if it occurs on multiple nodes or across all nodes within the same cluster) 

## > Check the state of the VMs

- Choose the **Datacenter > VMs > Virtual Machines** :   
  Check the value of status is Normal 
- Validation: All the VMs must be in « Normal » state  
  (In the case of a « Warning » state, there should be no incident reported, except if it occurs on multiple nodes or across all nodes within the same cluster) 

## > Check the state of the Datastores

- Choose the **Datacenter > Datastores > Datastores**:  
  Check the value of status is Normal
- Validation: All the Datastores must be in « Normal » state   
  (In the case of a « Warning » state, there should be no incident reported, except if it occurs on multiple Datastores or across all Datastores within the same cluster) 

## > Checking the performance of the hosts

- Choose the **Datacenter > Select the wanted cluster >  Monitor > vSphere DRS > CPU Utilization & Memory Utilization** : Verify the value for each node in each cluster does not exceed 80%) 

- Validation: Memory and CPU usage should not exceed 80%. 

## > Run the the Skyline Health test

- Choose the **Datacenter > Select the wanted cluster > Monitor > vSAN > Skyline Health** : Verify that no critical error exists) (No health score for satellite) 

- Value range:
  - Value range 81-100 = On Target/Healthy. 
  - Value range 61-80 = Degraded health. 
  - Value range 0-60 = Unhealthy. 

- Validation: The score must not be less than 80. 

## Send email and Create tickets:

Send a summary email regarding the state of the VXRAIL infrastructure using template in this folder MCO ACT - VxRail Infrastructure Health on ddmmyyyy) 

A ticket must be created and assigned to Support. The subject of the ticket depends on the failed metric and the type of error) 

Status of the Node
- If there is a service failure or a connection issue with the vCenter
```
Incidents/Infrastructure/Virtualisation/VMware/ESxi _Services et Agents 
```
- If the failure occurs at the physical layer of the node) : 
```
Incidents/Infrastructure/Virtualisation/VxRail/Hardware 
```
 

Status of VMs

- If the failure occurs at the ESXi layer
```
Incidents/Infrastructure/Virtualisation/VMware/ESxi _Services et Agents 
```
- Other cases: 
```
Incidents/Infrastructure/Virtualisation/VM bloquée	 
```
 

Status of Datastore:

- If the failure is due to saturation 
```
Incidents/Infrastructure/Virtualisation/VMware/Saturation du DataStore 
```
- If the failure occurs at the ESXi layer
```
Incidents/Infrastructure/Virtualisation/VMware/ESxi _Services et Agents 
```

Performance of the Hosts
```
Incidents/Infrastructure/Virtualisation/VMware/ESxi _Services et Agents 
```
 
Status of vSAN 
```
Incidents/Infrastructure/Virtualisation/VMware/ESxi _Services et Agents 
```
 
If the vCenter is unreachable 
```
Incidents/Infrastructure/Virtualisation/VMware/VCenter injoignable 
```

![alt text](</assets/2024/02/Screenshot_2024-02-15_105850.png>)