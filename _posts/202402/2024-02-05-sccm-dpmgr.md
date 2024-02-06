---
layout: post
title: "🛠️ SCCM: "
categories: IT SCCM
tags: [tshoot, sccm]
author: thu4nvd
---

## Symptoms
- Error message went to SCOM system as below:
  ```
  Distribution manager on the site server failed to access network. Event Description: On 31/01/2024 19:03:18, component SMS_DISTRIBUTION_MANAGER on computer BCNSSRV148.xxx.COM reported: Distribution Manager failed to connect to the distribution point. Possible cause: Distribution Manager cannot access the distribution point machine because of access permissions issues.  

  Solution: Make sure that the site server machine account or Site System Installation account has administrative permissions on the distribution point machine. Retry Interval is 30 minutes, number of retries left is 24.

  Date de réception : 31/01/2024 - 18:05:26
  Paramètre : MCM Distribution manager fails to access network Event Rule
  Object_Class : ConfigMgr Server Component
  Object : ConfigMgr Distribution Manager
  ```

## Check steps
- Access server then check the Event Viewer:
  ![alt text](</assets/2024/02/Screenshot 2024-02-06 175258.png>)
- Please note the message come from the same `Task Category` and `Source`
- The message shows:
  ```
  On 06/02/2024 11:05:25, component SMS_DISTRIBUTION_MANAGER on computer BCNSSRV148.x-x.COM reported: Distribution Manager successfully distributed package "PAP00110" to distribution point "["Display=\\bcnvsrv844.x-x.com\"]MSWNET:["SMS_SITE=PAP"]\\bcnvsrv844.x-x.com\".
  ```
  
## Solution:   
- That means the connection has been auto established.
- If not, please check the firewall and connection from Distribution Point to the Site Server.

