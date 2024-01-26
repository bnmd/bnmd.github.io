---
layout: post
title: "🛠 Veeam issue: Job failed due to VM recovery checkpoint"
date: 2023-01-26 04:47:00.000000000 -08:00
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
permalink: "/veeam-failed-vm-checkpoint/"
---

## Symptom
* Backup Job failed error:  
  
  ![Alt text](../assets/2024/v-chkpoint1.png)


* Error message: 
  
  > Failed to create VM recovery checkpoint...

## Cause

* The cause of the issue due to the VM is not merge disk automatically

## Solution
* Live migrate the VM to another Host
  
  ![Alt text](../assets/2024/v-chkpoint2.png)
  ![Alt text](../assets/2024/v-chkpoint3.png)

* If live migrate the VM not fix the issue -> Reboot the VM 
