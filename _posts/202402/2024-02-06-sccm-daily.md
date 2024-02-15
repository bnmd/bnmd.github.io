---
layout: post
title: "📃 SCCM Daily Report"
categories: IT SCCM
tags: [sccm, mecm]
author: thu4nvd
---

## Report description
- Name: MECM Daily Report
- Purpose: Maintenance tasks
- Frequency: From Monday to Friday  
- Duration: 60 minutes 
- Report no later than: 4pm

## Summary 

| Steps | Connect to |
|-----------|--------------|
| Verification of the MECM Site backup			| xxxSSRV148        |
| Inbox folders of MECM Site servers			| xxxSSRV148        |
| Primary site performances						| xxxSSRV148        |
| Distribution of failed packages				| xxxSSRV148        |
| Uninstalling MECM Agents from Tier 0 SRV		| MECM              |
| WSUS synchronization verification				| MECM              |
| Health of the site and its components			| MECM              |
| Monitor MECM Infrastructure via SCOM			| SCOM              |
| Software Updates evaluation verification		| xxxSDBA110        |
| SQL Always ON status							| Portail Reporting |
| Send Report Email                             |                   |

 
## Backup of the MECM Site:  

- Access to https://reportingdollaru.bcc.com/rapportchaine.php?chaine=SCCJSVAPNA&noeud=BYCNP06 to check all the status are ok 

  ![alt text](</assets/2024/02/Screenshot_2024-02-07_140859.png>)

- On ProxyAdmin, connect to xxxSSRV148 with S1 
- In the file explorer, go to **\\BYDATA02\SCCM_sauvegarde** and verify inside the folders « xxxSSRV148 », « xxxSDBA110 » and « xxxSDBA111 » that a current or previous day’s date folder has been created and that it contains data.  

  ![alt text](</assets/2024/02/Screenshot_2024-02-07_142744.png>)

- If there is any problem, open an Oasis ticket with the category **Incidents/infrastructure/MECM**  
- Stay connected to xxxSSRV148 

## Inbox folders of MECM Site servers 

- In the file explorer, go to 
  ```
  \\xxxSSRV148\e$\Microsoft Configuration Manager\Inboxes
  ```
  If this is your first connection, you may have to access this folder manually and to create a folder « Temp » 

- There are 10 folders to check. It is recommended to pin them for quick access. 

  - **compsumm**.box: Move the files older than 7 days to « Temp » 
  - **ddm**.box: Move the files older than 7 days to « Temp » 
  - **auth\dataldr.box\badmifs**: count the number of .MIF files with this PowerShell command. If the numbers exceed about 300, open an incident.  
    ```powershell
    Get-ChildItem -Path "E:\Microsoft Configuration Manager\inboxes\auth\dataldr.box\BADMIFS" -Include *.MIF -Recurse | ?{$_.LastWriteTime -ge (Get-date).AddDays(-2)} | Measure-object 
    ```
  - **hman**.box: Move the files older than 7 days to « Temp ». The parent folder must not contain more than 5 files. Do NOT touch the subdirectories. 
  - **sinv.box\badsinv**: count the number of .SID files with this PowerShell command. If the numbers exceed about 300, open an incident. 
    ```powershell
    Get-ChildItem -Path "E:\Microsoft Configuration Manager\inboxes\sinv.box\BADSINV" -Include *.SID -Recurse | ?{$_.LastWriteTime -ge (Get-date).AddDays(-2)} | Measure-object 
    ```
  - **statmgr.box**: Move the files older than 7 days to « Temp » 
  - **auth\statesys.box\corrupt**: Move the files older than 7 days to « Temp » 
  - **auth\ddm.box\regreq\bad_ddrs**: Move the files older than 7 days to « Temp » 
  - **COLLEVAL.box**: Move the files older than 7 days to « Temp » 
  - **ccrretry.box**: Move the files older than 7 days to « Temp » 

- In case of problems, open an Oasis ticket with the category « **Incidents/infrastructure/MECM** »  
- Stay connected to xxxSSRV148 

## Primary site performances 

- Open « **Task Manager** » column and check if both CPU and Memory are below 40% 
- If there is any problem, create an Oasis ticket with the category « **Incidents/infrastructure/MECM** »  
- Stay connected to xxxSSRV148 

## Distribution of failed packages 

- Open with Run as administrator « Windows Powershell »  
- To list the failed package distributions, execute the command 
  ```powershell
  E:\scripts\packages_error.ps1 -GridView
  ```
- Copy the results of the command and paste them in an Excel File. Use the formula « CONCAT » to assemble PAP**** and ****.***.bcc.COM 

  ![alt text](</assets/2024/02/Screenshot_2024-02-07_143252.png>)

- Copy the results and paste them in the file
  ```
  E:\scripts\Packages_a_redistribuer
  ```
  on xxxSSRV148 (delete the content before paste) 
- Execute the command 
  ```
  & 'E:\scripts\redistribute_packages on DPs.ps1' 
  ```
- For each package, you must get the value « True » as result of the redistribution   
  If you do not, report it and open an Oasis incident with the category « **Incidents/infrastructure/MECM** »   
- Log out from xxxSSRV148 

## Uninstalling MECM Agents on Tier 0 SRV 

- Open the MCM on S1 with ProxyAdmin 
- Go to « **Assets and Compliance / Overview / Device Collections / Servers / Server Localisation** ».  

  ![alt text](</assets/2024/02/Screenshot_2024-02-07_143552.png>)

- Double-click on SRV Tier 0 and sort by « Client ». 
  
  ![alt text](</assets/2024/02/Screenshot_2024-02-07_143658.png>)

- If an item has the value Yes for « Client » and Active for « Client Activity », note it in the report message  
- Stay connected to MECM 

## WSUS synchronization 

- Go to « **Monitoring / Overview / Software Update Point Synchronization** » and verify that all servers have the same Catalog Version, a Synchronization status Completed  
  - If the status is not Completed, the Last Synchronization Attempt should not be older than 24 hours. 
  ![alt text](</assets/2024/02/Screenshot_2024-02-07_143813.png>)

- If there is any problem, create an Oasis ticket with the category « **Incidents/infrastructure/MECM** »  
- Stay connected to MECM 

## Health of the site and its components 

- Go to « **Monitoring / Overview / System Status / Site Status** » and check if there is any critical status on xxxVSRV148 or xxxVSRV150. 

  ![alt text](</assets/2024/02/Screenshot_2024-02-07_144012.png>)
 
- Go to « **Monitoring / Overview / System Status / Component Status** » and check if there is any critical status.  
- If so, display messages to assess the criticality of the impact. 
- If the critical impact is confirmed or in case of doubt, create an Oasis ticket with the category « **Incidents/infrastructure/MECM** »  
- Close MECM 

## MECM Infrastructure via SCOM 

- Open the SCOM console:  
https://opsmgr.bcc.com/OperationsManager/#/monitoring/view  

- Go to « **Monitoring / System Center 2012 Configuration Manager** » 
- If there are critical alerts, make a Screenshot_to be inserted in the report email. 
- Close the SCOM console  

## Software Updates Evaluation 

- On ProxyAdmin, connect to xxxSDBA110 with S1 
- Open « **Microsoft SQL Server Management Studio** » and connect to « xxxSDBA110 » with S1 credentials.  
- Expand Databases and right-click on CM-PAP to select « New query » (You need to open two new queries) 
  - Query 1. Copy the text below and do Execute 
  ```sql
  SELECT  
     USS.LastScanPackageLocation as WUserver 
     ,count(distinct(S.Netbios_Name0)) as NbMachine 
     ,SN.StateName AS [LastScanState] 
     ,MAX(ScanTime) as MinScanTime 
     ,MAX(ScanTime) as MaxScanTime 
  FROM v_UpdateScanStatus as USS 
  INNER JOIN v_R_System S ON USS.ResourceID = S.ResourceID  
  INNER JOIN v_StateNames  as SN ON USS.LastScanState = SN.StateID and TopicType in (501) 
  INNER JOIN [dbo].[v_FullCollectionMembership] FC on FC.ResourceID=USS.ResourceID 
  where uss.LastScanPackageLocation <> 'https://xxxVSRV511.bcc.com:8531' 
  group by USS.LastScanPackageLocation,SN.StateName 
  having count(distinct(S.Netbios_Name0)) > 10 
  union 
  SELECT  
     USS.LastScanPackageLocation as WUserver 
     ,count(distinct(S.Netbios_Name0)) as NbMachine 
     ,SN.StateName AS [LastScanState] 
     ,MAX(ScanTime) as MinScanTime 
     ,MAX(ScanTime) as MaxScanTime 
  FROM v_UpdateScanStatus as USS 
  INNER JOIN v_R_System S ON USS.ResourceID = S.ResourceID  
  INNER JOIN v_StateNames  as SN ON USS.LastScanState = SN.StateID and TopicType in (501) 
  INNER JOIN [dbo].[v_FullCollectionMembership] FC on FC.ResourceID=USS.ResourceID 
  where uss.LastScanPackageLocation = 'https://xxxVSRV511.bcc.com:8531' 
  and s.Operating_System_Name_and0 like '%server%' 
  group by USS.LastScanPackageLocation,SN.StateName 
  --having count(distinct(S.Netbios_Name0)) > 4 
  order by 1 
  ```
  - Query 2. Copy the text below and do Execute 
  ```sql
  SELECT  
  SN.StateName AS [LastScanState] 
     ,count(distinct(S.Netbios_Name0)) as NbMachine 
     ,MAX(ScanTime) as MinScanTime 
     ,MAX(ScanTime) as MaxScanTime 
  FROM v_UpdateScanStatus as USS 
  INNER JOIN v_R_System S ON USS.ResourceID = S.ResourceID  
  INNER JOIN v_StateNames  as SN ON USS.LastScanState = SN.StateID and TopicType in (501) 
  INNER JOIN [dbo].[v_FullCollectionMembership] FC on FC.ResourceID=USS.ResourceID  
  group by SN.StateName 
  having count(distinct(S.Netbios_Name0)) > 10 
  order by 2 desc 
  ```

- Check the results of both queries 
  - Query 1: xxxSSRV150 – xxxSSRV151 – xxxVSRV2066 – xxxVSRV511 
    ![alt text](</assets/2024/02/Screenshot_2024-02-07_144800.png>)

  - Query 2: Global 
    ![alt text](</assets/2024/02/Screenshot_2024-02-07_144808.png>)

- If there are more than 30% failed for xxxVSRV511 or more than 10% on another server, create an Oasis ticket with the category « **Incidents/infrastructure/MECM** »  
- Log out from xxxSDBA110 

## SQL Always ON status 
- Access to this link https://reportingservices2016.bcc.com/Reports_infra/report/EXPLOITATION/TDB%20SQL%20Server/ETAT_ALWAYSON_AVAILABILITY_GROUPS  

- Go to Cluster Name **« xxxCDBA053** » and verify that all criterias are « Healthy », « Online », « OK », « OK » and « Conforme » 
  ![alt text](</assets/2024/02/Screenshot_2024-02-07_145002.png>)

- If one of them does not have the expected result, create an Oasis incident with the category « **Incidents/Infrastructure/MSSQL Server/Always On** »  

## Email Report  
- Use template to send email