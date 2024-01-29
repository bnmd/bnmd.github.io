---
layout: post
title: "🛠 Veeam issue: Job failed due to not enough space on Repository"
date: 2024-01-16 04:47:00.000000000 -08:00
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
permalink: "/veeam-failed-space/"
---

## Symptom
* Backup Job failed error:  
  
  ![Alt text](../assets/2024/vspace1.png)

* Error message: 
  
  >  Failed to upload disk. Agent failed to process method...   
  >  Backup location [...] is getting low on free disk space...


## Cause

* Run out of space on Repository.  
* Go to VBR console  
  Right click the Job -> properties

  ![Alt text](../assets/2024/vspace2.png)  

  RDP to Repository BYCSSVM009 to check disk usage on disk V


## Solution

- Delete old backup file out of retention policy
- There are 24 files backup that out of Retention Policy (21). So, delete the oldest full backup file  

  ![Alt text](../assets/2024/vspace3.png)

- Relaunch the backup job.