---
layout: post
title: "🛠 Veeam issue: Failed to connect to server - port 11731"
date: 2023-01-30 04:47:00.000000000 -08:00
type: post
parent_id: '0'
published: true
status: publish
categories:
- IT
- Veeam
tags: [tshoot, veeam]
author: thu4nvd
permalink: "/veeam-failed-prod-checkpoint/"
---

## Symptom
* Backup Job failed error:  
  
  ![Alt text](../assets/2024/01/conn1.png)


* Error message: 
  
  > Failed to connect to server - port 11731

## Cause

* This kind of issue 11731 due to the server is unreachable

## Solution

* Make sure the server is reachable again.   
  - check VPN, 
  - Firewall, 
  - status of network at site, 
  - idRAC, 
  - ask local support for help, 
  - migrate VM, 
  - check NIC of VM, etc...
* Relaunch the job -> Successful.