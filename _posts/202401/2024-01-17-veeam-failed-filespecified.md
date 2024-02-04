---
layout: post
title: "🛠 Veeam issue: Job failed due to error managed session"
date: 2024-01-15 04:47:00.000000000 -08:00
type: post
parent_id: '0'
published: true
status: publish
categories:
- IT
- Veeam
tags: [tshoot, veeam]
author: thu4nvd
---

## Symptom
* Backup Job failed error:  
  
  ![Alt text](/assets/2024/01/vsession1.png)


* Error message: 
  
  > Error: Managed session xxxxxxx-xxxx-xxx has failed

## Cause

* Unknown: due tu functionality of Veeam Agent service.

## Solution

- Restart Veeam service -> then retry the job

  > Veeam Agent for Microsoft ...
  ![Alt text](/assets/2024/01/vsession2.png)

-	If the issue still persists -> Reboot the server
