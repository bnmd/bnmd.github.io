---
layout: post
title: "🛠 Veeam issue: Job failed due to import backup path"
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
permalink: "/veeam-failed-import/"
---

## Symptom
* Backup Job failed error:  when rescan the Repository
  
  ![Alt text](../assets/2024/01/v-imp1.png)


* Error message: 
  
  > Failed to import backup path V:...

* Still able to access disk V from within Veeam Console:  

  ![Alt text](../assets/2024/01/v-imp2.png)
  ![Alt text](../assets/2024/01/v-imp3.png)

## Cause

* The problem is due to the Veeam service on VBR xxxx being stuck (CPU overloaded)

## Solution
* After killing the stucked session and restarting the Veeam service.   
  
  ![Alt text](../assets/2024/01/v-imp4.png)

* End process “Veeam.backup.manager” trong Task Manager

* Or use CLI with Administrator Permission:
* Command to kill veeam service  
  
  ```powershell
  taskkill /F /im Veeam.Backup.Service.exe
  ```

* If above command did not work, then use below:  
  
  ```powershell
  taskkill /F /im Veeam.Backup.Manager.exe
  ```
* Start veeam Backup service in Services Console
* Relaunch the backup job : The backup was successful  