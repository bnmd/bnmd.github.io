---
layout: post
title: "⚙️ Set up Portable environment DEV on Windows 11"
categories: IT Dev
tags: [dev, env, portable]
author: thu4nvd
---


## Context
- OS: Windows 11 Enterprise  
- Has no Administrator Account   
- Need to use bundle of portable programs jto set the Dev-Env  

## Git portable 
- Download git portable from site:   
  https://git-scm.com/   
- Then run executable file and put it in the target folder- i.e : 
  ```
  C:\ProgPortables\PortableGit
  ```
   
## Python portable
- Download portable Python 3 form site:   
  https://www.portabledevapps.ne-download-python-portable-3.9.php 
- Run exe file to extract to target folder: i.e: 
  ```
  C:\ProgPortables\Python-Portable-3.9.6
  ```
   
## VS Code portable 
- Download and install VS Code from:  
  https://code.visualstudio.com/download
- Install it as guide in the official site (easy)

## Set environmental variable 
- Run command to set the env variable for user (not for system):
   ```
   setx path  "C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\ProgramFiles\dotnet\;C:\Windows\system32;C:\Users\t.vuduc\AppData\Local\Microsoft\WindowsApps;C:\ProgPortables\MicrosoftVSCode\bin;C:\ProgPortables\PortableGit\bin;C:\ProgPortables\Python-Portable-3.9.6\;C:\ProgPortables\Python-Portable-3.9.6\apps;C:\ProgPortables\Python-Portable-3.9.6\apps\DLLs;C:\ProgPortables\Python-Portable-3.9.6\apps\Lib;C:\ProgPortables\Python-Portable-3.9.6\apps\Scripts;C:\ProgPortables\Python-Portable-3.9.6\apps\Lib\site-packages\PyQt5"
   ```
- Below is the clear list of paths need to be add into the variable %PATH%
   ```
   C:\Windows
   C:\Windows\System32\Wbem
   C:\Windows\System32\WindowsPowerShell\v1.0\
   C:\Windows\System32\OpenSSH\
   C:\ProgramFiles\dotnet\
   C:\Windows\system32
   C:\Users\t.vuduc\AppData\Local\Microsoft\WindowsApps
   C:\ProgPortables\MicrosoftVSCode\bin
   C:\ProgPortables\PortableGit\bin
   C:\ProgPortables\Python-Portable-3.9.6\
   C:\ProgPortables\Python-Portable-3.9.6\apps
   C:\ProgPortables\Python-Portable-3.9.6\apps\DLLs
   C:\ProgPortables\Python-Portable-3.9.6\apps\Lib
   C:\ProgPortables\Python-Portable-3.9.6\apps\Scripts
   C:\ProgPortables\Python-Portable-3.9.6\apps\Lib\site-packages\PyQt5
   ```

## Verification 
- Run command to verify, in whatever place in the system
  ```
  python --version
  git --version
  ```
- Open VScode then check in the console (Ctrl + ` )
   
## Notes 
- If you got unexpected message when run command `python --version` as below:  
   ```
   Python was not found; run without arguments to install from the Microsoft Store, or disable this shortcut from Settings > Manage App Execution Aliases.
   ```
  Then please turn off the setting of Manage App Execution Aliases then it will got the expected result:  
  ```
  C:\workspace\test-prj> python --version
  Python 3.9.6
  PS C:\workspace\test-prj> git --version
  git version 2.43.0.windows.1
  ```
 
## Configure SSH access to github
- For the first time git-push to remote repo (run command in the VSCode), it will request you to create credentials and log in to Github via browser, the procedure is quite strait-forward.

## Reference: 
- Stackoverflow [post](https://stackoverflow.com/questions/9546324/adding-a-directory-to-the-path-environment-variable-in-windows)
