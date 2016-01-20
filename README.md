# Chef Essentials

This is a repo for developing Chef Essentials.

The latest published version of these training materials are located as follows:

Chef-Essentials-Instructor-Kit, which contains

•	All 15 PPT slide decks, which you should teach from.

•	Instructor Guide for you to learn from, practice with, and perhaps use as reference while teaching. (The instructor guide contains speaker notes, instructor notes, and Appendix Z. Appendix Z contains lab setup info.)

•	Participant Guide, which is the same as the Instructor Guide sans instructor notes and Appendix Z.

•	README.

https://opscode.box.com/s/bx51ra6nk8r25f1c869xc5xkxckcxpte

Here is the link to only the Chef-Essentials-Participant-Guide. This is the link that will be in the student's confirmation email so some students may already have it prior to class starting.

https://opscode.box.com/s/75s0s971d7z5hdmtb6ua0os2vekf7zz6


## Modules

1. Intro
2. Resources
3. Cookbooks
4. chef-client
5. Testing Cookbooks6
6. Details About the System
7.	Desired State and Data
8.	Workstation Installation
9.	The Chef Server
10.	Community Cookbooks
11.	Managing Multiple Nodes
12.	Roles
13.	Search
14.	Environments
15.	Further Resources

## Publishing

Video on how to export the content to a Participant guide: https://drive.google.com/file/d/0B4WmSTt8VtdKZDY5RnhIWVVYZkk/view?usp=sharing

## Workstation Setup

The first series of modules focus on getting learners engaged with the content as quickly as possible. A workstation is provided to the learners.

This workstation is currently being managed as a Amazon Machine Instance (AMI). This AMI is managed by Chef through the Training AWS Account.

The workstation and the nodes are Windows 2012R2 instances with the following credentials:

```
user:     Administrator
password: Cod3Can!
```

The AMI was generated with the following actions:

> NOTE: The creation of the image is a manual process of executing a number of steps and then saving that current working instance as an image.

* Installed the basic components through the Nordstrom's ChefDK bootstrap script

```
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/Nordstrom/chefdk_bootstrap/master/bootstrap.ps1).Content | Invoke-Expression
```

* Enabled powershell script execution
* Changed the password to "Cod3Can!"
* Installed the Atom FoodCritic Linter
* Installed the Atom Rubocop Linter
* Write a `kitchen-template.yml` to the "\\Users\\Administrator" that contains the following [content](https://github.com/chef-training/chef-essentials-windows/blob/master/kitchen-template.yml).
* Install the kitchen-ec2 gem `chef gem install kitchen-ec2`
* Enable remote administration via WINRM

```
# WinRM
write-output "Setting up WinRM"
write-host "(host) setting up WinRM"

cmd.exe /c winrm quickconfig -q
cmd.exe /c winrm quickconfig '-transport:http'
cmd.exe /c winrm set "winrm/config" '@{MaxTimeoutms="1800000"}'
cmd.exe /c winrm set "winrm/config/winrs" '@{MaxMemoryPerShellMB="1024"}'
cmd.exe /c winrm set "winrm/config/service" '@{AllowUnencrypted="true"}'
cmd.exe /c winrm set "winrm/config/client" '@{AllowUnencrypted="true"}'
cmd.exe /c winrm set "winrm/config/service/auth" '@{Basic="true"}'
cmd.exe /c winrm set "winrm/config/client/auth" '@{Basic="true"}'
cmd.exe /c winrm set "winrm/config/service/auth" '@{CredSSP="true"}'
cmd.exe /c winrm set "winrm/config/listener?Address=*+Transport=HTTP" '@{Port="5985"}'
cmd.exe /c netsh advfirewall firewall set rule group="remote administration" new enable=yes
cmd.exe /c netsh firewall add portopening TCP 5985 "Port 5985"
cmd.exe /c net stop winrm
cmd.exe /c sc config winrm start= auto
cmd.exe /c net start winrm
cmd.exe /c wmic useraccount where "name='chef'" set PasswordExpires=FALSE
```
