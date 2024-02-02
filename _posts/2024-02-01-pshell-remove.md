---
layout: post
title: "[>_] PowerShell to Delete a Folder"
date: 2024-02-01 04:47:00.000000000 -08:00
type: post
parent_id: '0'
published: true
status: publish
categories:
- IT
- Powershell
tags: [pshell]
author: thu4nvd
permalink: "/pshell-remove/"
---

## How to Delete a Folder Using PowerShell?

As a system administrator or a regular user, there are times when you need to delete folders on your computer. While deleting a folder manually is a straightforward process, it can be time-consuming if you have multiple folders to delete. However, with PowerShell, you can automate the process and save time.

To begin, open the Windows Start menu, type “PowerShell” into the search bar, and open it as administrator (Otherwise, You may run into the “access to the path is denied” error!). To delete a folder in PowerShell, you can use the Remove-Item cmdlet with the -Recurse parameter. This cmdlet will need to specify the path of the folder you want to delete using the -Path parameter. Here’s an example of how to use it:

```powershell
Remove-Item -Path C:\path\folder -Recurse
```

This will delete the folder and all of its contents, including subfolders and files. You can verify that everything was deleted by going to your original folder location using Windows File Explorer and making sure that it has been removed completely from there. Please note, the Remove-Item cmdlet can also be used for deleting many other items, including files, folders, registry keys, variables, aliases, and functions.

## Deleting Multiple Folders using PowerShell script

PowerShell’s strength lies in its ability to handle multiple items at once. If you need to delete multiple folders simultaneously, simply provide their paths separated by a comma:

```powershell
Remove-Item -Path C:\Temp\MyFolder1, C:\Temp\MyFolder2 -Recurse
```

This command will delete both MyFolder1 and MyFolder2, along with their contents. Similarly, to delete multiple folders and contents without confirmation, use:

```powershell
#Parameter
$Directories = "C:\Temp\Logs", "C:\Temp\Backups", "C:\Temp\AppLogs"
 
#Delete files in each directory
ForEach ($Dir in $Directories) {
    Remove-Item -Path $Dir -Recurse -Force
}
```

## How to force delete a folder in PowerShell?

Sometimes, you may encounter an error when trying to delete a folder with PowerShell. This can happen if the folder contains read-only files or if another program is using it.

You can use the -Force parameter to delete the folder and its contents without prompting for confirmation. For example:

```powershell
Remove-Item -Path "C:\Users\Thomas\Desktop\Unused" -Recurse -Force
```

The above command will delete the folder and its contents without prompting for confirmation.


## PowerShell Delete Folder If Exists

Here is an example of how to delete a folder in PowerShell if it exists:

```powershell
#Folder Path
$FolderPath = "C:\Temp\New"
 
#Check if folder exists
If (Test-Path $FolderPath) {
    # Folder not exist, delete it!
    Remove-Item -Path $FolderPath -Recurse
    Write-host "Folder Deleted at '$FolderPath'!" -f Green
}
Else {
    Write-host "Folder '$FolderPath' does not exists!" -f Red
}
```

## Delete a Folder in the Current Directory with PowerShell

Once you have opened PowerShell, use the cd command to change directories to where the folder you want to delete is located. For example, if your folder is called “NewFolder” and it is located in “C:\Users\YourUserName\Documents”, then use the following command: cd C:\Users\YourUserName\Documents.

You can also use the rd or rmdir command as you do in command prompt, which is an alias for Remove-Item, to delete a folder and its contents. For example, to delete a Folder “New” in the current directory with all its contents without prompting for confirmation, use:

```powershell
rd .\[Your Folder Name] -Recurse -Force
```

## PowerShell to delete a folder and subfolders

If you want to delete a folder and its subfolders, you can use the Remove-Item command with the -Recurse parameter. For example, to delete the folder “C:\Folder1” and all its subfolders, you can run the following command:

```powershell	
Remove-Item -Path "C:\Folder1" -Recurse
```

The Remove-ItemRecurse command is used to delete a folder and all its contents recursively from specified locations. This means that it will delete all the subfolders and files in the folder.

## Delete all Files and Sub-Folders from a Folder using PowerShell

How to delete all files and sub-folders from a Folder without deleting the given folder using PowerShell? To delete all files and folders from a specific folder using PowerShell, you can use the Remove-Item cmdlet with the -Recurse and -Force parameters.

Here’s an example of how to use this cmdlet to delete all files and folders from a folder called “C:\Temp”:

```powershell	
Remove-Item C:\Temp\* -Recurse -Force
```

The * wildcard tells PowerShell to delete all files and folders in the “C:\Temp” folder. The -Recurse parameter tells PowerShell to delete the contents of the folder, including any subfolders and files. The -Force parameter tells PowerShell to delete the files and folders without prompting for confirmation.


## Delete Specific contents of a folder

If you want to delete the contents of a folder without deleting the folder itself, you can use the Remove-Item command with the -Exclude parameter. This parameter allows you to specify the files or folders that you want to exclude from the delete operation. For example, to delete all the files in the folder “C:\Folder1” except for the file “file1.txt”, you can run the following command:

```powershell	
Remove-Item -Path "C:\Folder1\*" -Exclude "file1.txt" -Recurse
```

Similarly, You can use the “-Include” switch to include only specific files or file types for deletion. Similarly, you can filter specific file types and delete file (E.g., Txt files) with wildcard characters.

```powershell	
# Delete only .txt files in a directory
Remove-Item -Path "C:\Temp\*" -Filter "*.txt"
```

In this example, -Filter “*.txt” tells Remove-Item to only delete items that match the filter, which in this case is all .txt files. To include multiple files in deletion, use:

```powershell	
# Delete only .txt and .docx files in a directory and its subdirectories
Remove-Item -Path "C:\Temp\Logs\*" -Include "*.txt","*.docx" -Recurse
```

In the above script, -Include “.txt”,”.docx” tells Remove-Item to only delete items that match the include patterns, which in this case are all .txt and .docx files. The -Recurse parameter tells it to look in all subdirectories of the specified path for child items.

## Deleting Folders Based on a Condition

PowerShell can also delete folders based on specific conditions. For example, you might want to delete all folders that haven’t been modified in the last seven days. Here’s how you could do this:

```powershell	
Get-ChildItem -Path C:\Temp -Recurse -Directory | Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-7) } | Remove-Item -Recurse -WhatIf
```

This command retrieves all directories in C:\Temp using Get-ChildItem (Aliases: Dir, GCI, or LS), which were last modified more than seven days ago, and pipe them to the Remove-Item cmdlet to delete them. The -Whatif parameter is added to preview the operation without actually doing the delete operation.

## Precautions to take before deleting folders with PowerShell

Before you start deleting folders with PowerShell, it is important to take some precautions to avoid accidentally deleting important files or folders. Here are some precautions that you should take:

- Double-check the folder path to ensure that you are deleting the correct folder.
- Back up important files and folders before deleting them.
- Test the delete operation on a test folder to ensure that it works as expected. Use -whatif switch to preview the results.

## Wrapping up

Deleting a folder using PowerShell is a simple process that can be completed using a few different methods. PowerShell is an extremely powerful tool that can greatly simplify your life when dealing with file system tasks. By harnessing the power of cmdlets like Remove-Item, you can handle complex scenarios with ease. Whether you’re dealing with a single folder, multiple directories, or complex conditions, PowerShell offers an efficient and reliable solution.

This article showed you how to delete a folder using PowerShell. Please note, all these methods completely remove the Folder without sending it to the recycle bin.

## Frequently Asked Questions (FAQs)

* How to delete Empty Folders using PowerShell?

  Use the Get-ChildItem cmdlet in combination with Remove-Item to recursively delete all empty directories within a specified directory:
  ```powershell
  Get-ChildItem -Path "C:\Temp\Logs" -Recurse -Directory | Where-Object { (Get-ChildItem -Path $_.FullName -Recurse -File -EA SilentlyContinue | Measure-Object).Count -eq 0 } | Remove-Item -Recurse -Force
  ```

* What is the command to delete a folder in CMD?

  To delete a folder with subfolders and files in CMD (Command Prompt), you can use the rmdir (or equivalently rd) command. Here’s an example:
  ```powershell
  rmdir /S /Q "C:\path\to\your\directory"
  ```
  Here, /S removes the specified directory and all of its subdirectories, including all files. /Q runs in quiet mode, which means you won’t be asked to confirm the deletion.

* How do I delete all folders and files in a folder in PowerShell?

  To delete all files and folders recursively in PowerShell, use the Remove-Item cmdlet. Here’s an example:
  ```powershell
  Remove-Item -Path "C:\Temp\Logs\*" -Recurse -Force
  ```

* How do I delete a file in the PowerShell command line?

  You can delete a file in PowerShell using the Remove-Item cmdlet.
  ```powershell
  Remove-Item -Path "C:\Temp\LogFile.txt" -Force
  ```
  The -Force parameter helps to delete read-only files and suppress any confirmation prompts.

## Reference:

  Read more: https://www.sharepointdiary.com/2020/02/how-to-delete-a-folder-in-powershell.html#ixzz8QZ3ULgIa
