---
layout: post
title: "⚙ SCCM Daily maintenance tasks"
date: 2024-02-02 02:02:00.000000000 +7:00
type: post
parent_id: '0'
published: true
status: publish
categories:
- IT
- SCCM
tags: [sccm]
author: thu4nvd
---

Microsoft System Center Configuration Manager (SCCM) is a powerful tool for managing large-scale enterprise systems. It is essential for IT administrators to perform regular maintenance tasks in order to ensure that SCCM is functioning optimally and to prevent any potential issues from arising. In this blog post, we will discuss the daily maintenance tasks that should be performed in order to keep SCCM running smoothly.

## Check overall site server health and hardware performance

The first step in SCCM’s daily maintenance tasks is to check the health of the SCCM site server itself. We suggest starting by simply looking at the overall resource consumption in the Task Manager.

* Verify RAM usage. Ensure that there’s no overuse. The most RAM-consuming application should be SQL (if running locally). If there’s no more RAM available, consider adding more RAM. If all the memory is used by SQL, ensure that you follow our recommendation.

  ![SCCM Daily Maintenance Tasks](/assets/2024/02/image-1.png)

* Verify CPU usage and ensure that no process is taking 100% CPU. The process that is more likely to consume CPU is SQL (if your database is running locally) and IIS Worker Process (w3wp.exe).
  ![CPU Usage](/assets/2024/02/image-2.png)

  If your IIS Worker Process is using all CPU, we suggest that you read our post about Software update maintenance best practice

* Verify all disk-free space and ensure that there’s enough free space left. Here are the main things to check which consume lots of disk space :  
  - SQL Database size
  - SQL Logfiles size
  - SCCM ContentLib
  - IIS Logs files (covered in this article below)
  - WSUS download directory (WSUSContent)  
  
  If SQL is causing issues, we recommend that you review your SQL configuration.

## Monitor site status

This can be done by opening the SCCM console and navigating to the Monitoring workspace. From there, you can view the status of the site server and ensure that it is running properly. If any issues are found, they should be addressed as soon as possible to prevent them from becoming more serious problems.

- In the SCCM Console, go to **Monitoring\ System Status \ Site Status**
- Ensure that all components are in a functioning state

  ![SCCM Daily Maintenance Tasks](/assets/2024/02/image-3.png)

- If a component is having an error or warning, simply right right-click on it and select **Show Message / All**
  
  ![Alt text](/assets/2024/02/image-4.png)

## Monitor software distribution

SCCM is used to distribute software to clients across an enterprise network. You should regularly monitor the software distribution process to ensure that all software packages are being deployed correctly and in a timely manner. Any issues with software distribution should be addressed promptly to prevent any disruption to the users.

- In the SCCM Console, go to **Monitoring \ Distribution Status \ Content Status**
- Check the Compliance 100% and fix any distribution issues on affected distribution points. To have more details, right-click your application or package and select View details
  ![Alt text](/assets/2024/02/image-5.png)

## Monitor client health

The health of the SCCM clients is also an important factor in the overall health of the SCCM infrastructure. You should regularly check the client’s health status to ensure that all clients are functioning properly and that they are sending inventory, receiving updates and software deployments in a timely manner. If any issues are found with client health, they should be addressed as soon as possible to prevent any disruption to the users.

To check the client’s health, go to **Monitoring \ Client Status \ Client Health Dashboard**
  ![Alt text](/assets/2024/02/image-6.png)

From there, you have tons of metrics to identify potential problems.

Ensure that your installation account has the necessary right for your client’s installation. Also, ensure that your clients are up-to-date following an SCCM update.

## Backup SCCM databases

Regularly back up the SCCM databases to ensure that data is not lost in the event of a disaster. You should schedule regular backups of the SCCM databases and store them in a secure location to ensure that they can be restored in the event of an emergency.

Validate that the SCCM Backup Maintenance task is enabled:

- In the SCCM Console, go to **Administration \ Site Configuration \ Sites**, click on Site Maintenance at the top

  ![Alt text](/assets/2024/02/image-7.png)

- Ensure that the task is enabled and configured correctly.

  ![Alt text](/assets/2024/02/image-8.png)

Check the **Smsbkup.log** for a detailed backup report. Also, ensure that your backup agent is backing up the folder in which you configure it.

## Check WSUS and ADR synchronization status

We suggest checking for the WSUS synchronization status. If WSUS is failing, you won’t be able to deploy the latest windows patches to your clients.

- Go to **Monitoring \ Software Update Point Synchronization Status**
  Ensure that there’s no error in the console and review WSUSCtrl.log and wsyncmgr.log
  
  ![Alt text](/assets/2024/02/image-9.png)

  Keep an eye on your Automatic deployment rules and ensure that they are successful. Review the RuleEngine.log if any errors.

- Go to **Software Library \ Software Updates \ Automatic Deployment Rules** 
  
  ![Alt text](/assets/2024/02/image-12-1021x196.png)


## Clean up IIS logs

After a couple of weeks of running an SCCM server, you may get the C:\ drive full. What are the best practices to save disk space on an SCCM server? The first thing we check is how much space is filled by SCCM IIS log files. Usually, it takes a couple of GB on the drive. It’s an absolute must to implement solutions to delete SCCM IIS log files from your primary server.

We wrote a complete article on how to manage IIS log files.

## Archive completed deployment

Why keeps completed or months-old deployment? It only brings useless data to your SCCM server. You can review all your deployment in the console under **Monitoring \ Deployments**

  ![Alt text](/assets/2024/02/image-10.png)

We wrote a script that can help you delete old SCCM deployments.

## Review collection evaluation

Since SCCM 2010, the functionality of Collection Evaluation Viewer is integrated into the SCCM console. This functionality provides administrators with a central location to view and troubleshoot the collection evaluation process. Before you would have to use the CEViewer from the SCCM tool folder but now, there’s no reason to check this on a regular basis.

You may wonder why you should care about this. Collections in Configuration Manager is a resource-intensive task, and some best practices need to be followed.

We wrote a complete blog post on collection management tips. Check it out.

  ![Alt text](/assets/2024/02/image-11.png)

## Review OSD Task Sequence deployments and collections

Another SCCM Daily Maintenance Task we recommend is your operating system deployment collections. You probably just drop devices in there and never remove them. We recommend removing any collection memberships that are no longer needed and also, deleting any empty collections and collections without members. This will reduce the load on your collection evaluation process.

## SCCM Daily Maintenance Tasks – Backup Custom Reports

If you are creating custom reports, you must manually back up these in case of a disaster.

We used this Powershell script numerous times to do so. Just run this command on your reporting point and all your SCCM reports will be back up.

  ![Alt text](/assets/2024/02/SCCM-Windows-11-Report-MEMCM-1-2048x649.png)

## WSUS Maintenance

One last tip to complete this blog post. This is not a daily task, plan to add step monthly to maintain your WSUS and Software Update Point health. This will ensure the good functionality of your Software updates tasks.

## Reference 

[Blog: System Center Dudes](https://www.systemcenterdudes.com/sccm-daily-maintenance-tasks/)