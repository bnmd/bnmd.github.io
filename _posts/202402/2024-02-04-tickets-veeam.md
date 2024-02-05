---
layout: post
title: "✅ Tickets 2024-01 Veeam Backup"
categories: IT Veeam
tags: [veeam, tickets]
author: thu4nvd
---

Collection of tickets monthly  
Time: 2024-01 and previous months.  
Category: Veeam Backup Replication

## I240103_0001 : Failed to select Gateway 

* Issue: Error message of the backup job:  
  ```
  Unable to allocate processing resources. Error: Failed to select a gateway server and acquire file sessions
  ```
  
* Solution: 
  - Check the current gateway of the backup job/server.
  - Change the good gateway server will resolve the issue.

## I231031_0480 : Connection with DC 

* Issue: Job failed with unknow message -> have to check logs.
* Solutioin:  
  Après le reboot, le probleme persiste, dans le log event il y a des message d'erreur NETLOGON indiquant le problème de communication avec les DC.

  ![alt text](/assets/2024/02/netlogon.png)

## I230730_0030 : 

* Issue: 
  Please note that we have an issue for the VEEAM backup, no one amongst BBIKSA local group's SMEA account is able to access the portal to restore files/folders.  
  Execution Timeout Error. This has been going on for the past few weeks, please see below screenshot error.  

  ![alt text](</assets/2024/02/2024-02-05 14_35_10-Window.png>)

* Solution:  
  Pb is resolved after helps Backup veeam support. 


## Some other solutions  

- Wait for job complete, sometimes it takes long time to complete the jobs. Verify speed, bandwidth, bottleneck, log of backup jobs. 
- Relaunch the job after verifying the conditions (space, creditals, AD connect, agent status, etc...)
- Most jobs are completed. the jobs failed due to password expired.
- Après le reboot, le probleme persiste, dans le log event il y a des message d'erreur NETLOGON indiquant le problème de communication avec les DC.
Pourriez-vous vérifier le bon site ID dans l'AD svp ?
- Probleme resolved,  For now No tape libraries are online
- Check backup job is disabled or not, is there still the need for backup.
- Pb is resolved by migration repository
- VMs exclus de la sauvegarde.
- Sauvegarde ok apres reboot du serveur
- Failed jobs are related to site VDR unreachable this morning.  
Now the link has been restored and the VDR is reachable again.
- La sauvegarde ne peut aboutir car instabilité de la connexion.
- SAP HANA - backup log status is now OK
- Backup job of the VM xxxVDBA097 is backup success on VBKP-xxxCVMW04-Application-Aware
- Le serveur prend bcp de temps pour l'autre sauvegarde; le debit est tres bas   
Le debit est maintenant ameliore; apres avoir change la gateway
- these vms have been migrated to vcenter and backed up well
- VM exclue de la sauvegarde
- the jobs failed due to CPU overload, the memory and CPU have been upgraded. 
- J'en ai discuté avec le service informatique local et il m'a dit que le serveur était en train de migrer vers Equans SI. JOB desactivé. 
- We don't need this backup job anymore
- Please insert a tape manually in the local rebot
- Tapes retired
