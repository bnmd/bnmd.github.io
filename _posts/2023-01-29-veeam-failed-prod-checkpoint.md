---
layout: post
title: "🛠 Veeam issue: Failed to create production checkpoint"
date: 2023-01-29 04:47:00.000000000 -08:00
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
permalink: "/veeam-failed-prod-checkpoint/"
---

## Symptom
* Backup Job failed error:  
  
  ![Alt text](../assets/2024/01/prod-ckp1.png)


* Error message: 
  
  > Failed to create production checkpoint ...

## Cause

* On SCVMM -> Server is still running
* But ping failed to that server.
  
  ![Alt text](../assets/2024/01/prod-ckp2.png)

## Solution
* Live migrate the VM to another Host
  
  
  ![Alt text](../assets/2024/01/prod-ckp3.png)
  
* Relaunch the job -> Successful.

  ![Alt text](../assets/2024/01/prod-ckp4.png)