---
layout: post
title: "🛠 SCOM Agent - clear cache"
date: 2024-01-28 04:47:00.000000000 -08:00
type: post
parent_id: '0'
published: true
status: publish
categories:
- IT
- SCOM
tags:
- tshoot
- scom
author: thu4nvd
permalink: "/scom-agent-cache/"
---

Trong quá trình monitor SCOM Agent bị disconnect bởi nhiều lý do, hay còn gọi là `grey-out`.  

## Symptom

* Khi check trên SCOM Console biểu tượng State bị grey out trở nên màu xám, khi đó SCOM Server không connect được với Agent.


## Cause

* Agent SCOM chưa started
* Agent SCOM vẫn chạy nhưng bị stuck -> clear cache. 


## Solution

* Run các command dưới đây để Check status của SCOM Agent và clear cache nếu cần:  

   ```powershell
   # Check status of HealthSerivce - SCOM Agent
   gsv HealthService 
   # Stop Health service in order to clear cache 
   net stop HealthService
   # Clear cache 
   Remove-Item –path "C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Health Service Store\*"
   # Check size of folder cache after clearing
   Get-ChildItem "C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Health Service Store"| Measure-Object -Property Length -sum
   # Start the service SCOM Agent 
   net start HealthService
   # Test network connection to the SCOM Server 
   tnc bcnvsrv3003 -p 5723
   ```

   Kết quả có thể như dưới đây 
   
   ```powershell
   PS C:\> Remove-Item –path "C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Health Service Store\*"
   PS C:\> Get-ChildItem "C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Health Service Store"| Measure-Object -Property Length -sum
   PS C:\> net start HealthService
   The Microsoft Monitoring Agent service is starting.
   The Microsoft Monitoring Agent service was started successfully.
   
   PS C:\>
   PS C:\> tnc bcnvsrv3003 -p 5723
   ComputerName     : bcnvsrv3003
   RemoteAddress    : 10.5.228.9
   RemotePort       : 5723
   InterfaceAlias   : Ethernet1
   SourceAddress    : 10.5.237.113
   TcpTestSucceeded : True
   
   PS C:\> date; hostname
   lundi 29 janvier 2030 04:28:37
   BCNVSRV934
   ```

## Reference 