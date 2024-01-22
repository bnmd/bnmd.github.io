---
layout: post
title: "🛠 Veeam issue: VSS Writer failed"
date: 2023-01-23 04:47:00.000000000 -08:00
type: post
parent_id: '0'
published: true
status: publish
categories:
- IT
- Veeam
tags:
- tshoot
- veeam
author: thu4nvd
permalink: "/veeam-vm-migration/"
---

# Veeam issue: Backup job failed due to VM Migration

## Symptom
* Backup Job failed error:  
  
   ![Alt text](../assets/2024/vm-mig1.png)

   ![Alt text](../assets/2024/vm-mig2.png)

* Error message: Failed to collect VM information. Failed to collect VM disk...


## Cause

* Backup job failed due to the VM is now in the status " Migration failed " in SCVMM  

   ![Alt text](../assets/2024/vm-mig3.png)

## Solution
* Repair the VM : The button Refresh is grayed out, so cannot refresh the VM -> We need to repair the VM  

  ![Alt text](../assets/2024/vm-mig4.png)

  ![Alt text](../assets/2024/vm-mig5.png)

* Relaunch the backup job  
