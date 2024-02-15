---
layout: post
title: "🙋‍♂️Veeam and File system ReFS"
categories: IT Veeam
tags: [veeam]
author: thu4nvd
---

## What's ReFS

In order to solve the problems of the NTFS file system, Microsoft introduced a new generation of the advanced file system - Resilient File System (ReFS), also known as "Protogon", in September 2012. 

ReFS is designed from NTFS code. Microsoft hopes it can have the maximum data availability and meet more data storage needs of users. And Windows is getting ReFS support.


Compared with the NTFS file system, the ReFS file system does improve reliability, especially for aging disks or when the machine is powered off due to movies. The reliability part comes from the underlying changes, such as the storage and update of file metadata. ReFS is compatible with the Storage Spaces spanned volume technology. When the disk fails to read and write, ReFS will perform system verification, detect these errors, and copy the file correctly.

However, although Microsoft has made a lot of efforts to design ReFS, and ReFS also has functions that NTFS does not have, it still has many shortcomings that need to be improved, and it cannot be a perfect substitute for NTFS for the time being.

## Question 1 : action plan to change NTFS - ReFS 

I use Veeam v10 to back up my Hyper-V VMs.   
My backup repository is a Win2016 server with local hard drives. The volume is formatted with NTFS. I would now like to convert the volume to ReFS to take advantage of e.g. block/fast clone.  
I have about 6 TB of data on the volume.  
My planned approach:  
- Stop all jobs
- Copying the data to an external media
- Format the volume with ReFS
- Copying the data back
- Reactivate the jobs

## Answer 1 

Yes, but you will have to also:
- edit through repository settings going next-next-next-finish style, for us to detect ReFS
- initiate another Active Full for all jobs, for us to start using Fast Clone

Check out Fast Clone limitations mentioned here.
You can left your old backup data on the external media.
Veeam is able to restore data from the external media without any problem.
You can import the backups or create a backup repo target to the folder on the external media.

Import Backups:
https://helpcenter.veeam.com/docs/backu ... ml?ver=100

Adding Backup Repo (normal process :-))
https://helpcenter.veeam.com/docs/backu ... ml?ver=100


![alt text](/assets/2023/02/Screenshot_2024-02-07_212350.png)

## Question 2: Best practice

I started reading through several very long threads on this forum about using ReFS for a Veeam repository. I had a hard time gleaning current best practice around using ReFS. I have a Nimble CS300 array that I will use to present a LUN to our physical proxy server which is running Windows Server 2016 build 1607.
- When I go to mount and format this LUN on the Windows server, what cluster size should I choose?
- We currently backup to a LUN presented to this same Windows 2016 server but this LUN is formatted NTFS. Are there different settings from within Veeam I should be using if the target is ReFS as opposed to NTFS?
- Lastly, is ReFS ready for production? Stable?

We backup a total of 5.5TB so the amount is not huge. Our new physical proxy server has 2 CPUs with 16 cores each running at 2.6GHz, 64GB RAM, and 10 gig network connection. It sounds like we are fine here
We use the following settings for our current backup job for Windows VMs to an NTFS formatted volume. When we go to ReFS, none of these should change, correct?
- Forever forward incremental with a 30 day retention
- Backup file health check once a month
- Removed deleted items after 14 days and defragment/compact full backup once a month
- Compression level optimal and Storage Optimization set to local target
- Change block tracking enabled
- Backup from storage snapshots enabled since we have a Nimble array
- Application aware processing is enabled
	
## Answer 2

1. We advice 64k as clustersize
2. No. Once you create a repository on that ReFS lun, veeam will detect it and use it if possible
3. Yes, and no. Yes: All patches applied, enough RAM/CPU to the server and not petabytes of data. Microsoft is still working on some fixes that will come but no idea when. The major issues however seem to be gone	

You don't need any changes. But you will have more flexibility around backup modes, for example enabling periodic synthetic fulls will not require extra disk space with ReFS. Which in turn removes the need to do periodic defragment/compact (as synthetic full does it implicitly).

Pretty much the only concern when switching to ReFS is the amount of RAM on the backup repository server, which you have plenty for your data size.


## Reference
- Convert ntfs to refs to take advantage of fast clone. 👉[Link](https://forums.veeam.com/microsoft-hyper-v-f25/convert-ntfs-to-refs-to-take-advantage-of-fast-clone-t72109.html)
- ReFS for repository, best practice? 👉[Link](https://forums.veeam.com/veeam-backup-replication-f2/refs-for-repository-best-practice-t52512.html)
- Veeam backup and ReFS formatted volume - possible issue? 👉[Link](https://community.spiceworks.com/topic/2343123-veeam-backup-and-refs-formatted-volume-possible-issue)
- ReFS vs NTFS: Which Is a Better File System? 👉[Link](https://www.easeus.com/knowledge-center/refs-vs-ntfs.html)