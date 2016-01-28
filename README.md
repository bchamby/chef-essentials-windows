# Chef Essentials

This is a repository for developing Chef Essentials for Windows.

## Abstract

Chef Essentials is a comprehensive instructor-led course covering the basic architecture of Chef, the use of Chef Development Kit (ChefDK), and associated tools. Development, engineering, and operations staff will learn to use Chef to automate the configuration, deployment, and management of server infrastructure. Participants will also learn how to test their configurations. Each of the core units in this course has hands-on exercises to reinforce the material. At the end of the course, students will have a code repository that can be used and modified to solve real business problems.

## Learner Requirements

Attendees need a network-enabled laptop with a Remote Desktop Client that supports RDP.

* Windows 7 with [Remote Desktop Connection](http://www.wikihow.com/Use-Remote-Desktop-in-Windows-7)

* Mac OS X 10.11 (El Capitan) with [Microsoft Remote Desktop](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12)

* Ubuntu 14.04 with [Remmina Remote Desktop Client](http://www.remmina.org/wp/)


It’s best that learners have some familiarity and comfort with the following:

* Writing code (of just about any flavor) in a text editor
* Working on the command line
* Basic system administration – installing packages, configuring those packages, starting services

## Agenda

1. Introduction
2. Resources
3. Cookbooks
4. chef-client
5. Testing Cookbooks6
6. Details About the System
7. Desired State and Data
8. Workstation Installation
9. The Chef Server
10.	Community Cookbooks
11.	Managing Multiple Nodes
12.	Roles
13.	Search
14.	Environments
15.	Further Resources

## Published Content

Video on how to export the content to a Participant guide: https://drive.google.com/file/d/0B4WmSTt8VtdKZDY5RnhIWVVYZkk/view?usp=sharing

The latest published version of these training materials are located as follows:

### Participant Guide

The participant guide is a PDF that contains the notes export from the content slides.

The content can be found here: NOT YET RELEASED

### Instructor Kit

* All 15 PPT slide decks for each module.

* Instructor Guide for you to learn from, practice with, and perhaps use as reference while teaching. The instructor guide contains the notes export from the content slides with additional instructor notes and lab setup instructions.

* Participant Guide

This content can be found here: NOT YET RELEASED

### Screencast Videos

This content can be found here: NOT YET RELEASED

## Known Issues

* MODULE 05 - There seems to issues with the Windows instances instantiated from AMI not being able to reach the metadata server. Test Kitchen will not work because the instance is not able to retrieve the IAM role credentials to instantiate test instances.

* MODULES 10-14 - There seems to issues with the Windows instances instantiated from AMI not being able to reach the metadata server. The cloud metadata is not able to be loaded into the node object. This make it so the node will return an empty set of data when using `node['cloud']` (modules 10-14). This will also make it impossible to use `knife winrm` because it is using the internal IP Address.

> On an Amazon EC2 instance you can verify if the metadata is available by executing the following command: `curl http://169.254.169.254/latest/meta-data/`

* MODULES 09-12 - Currently with Chef 12.5.1 there is an issue where the initial bootstrapping will hang. The Ruby process will appear unresponsive. The only workaround at the moment is to login to the instance, open Task Manager, kill the Ruby process, and then re-bootstrap the instance again.

## Workstation Setup

The first series of modules focus on getting learners engaged with the content as quickly as possible. A workstation is provided to the learners.

### Amazon Machine Instance

This workstation is currently being managed as a Amazon Machine Instance (AMI). This AMI is managed by Chef through the Training AWS Account.

*
https://github.com/chef-training/chefdk-image/blob/master/cookbooks/workstations/recipes/essentials.rb
 (
ami-cd95b3a7)

> This AMI was generated manually. Though in the future the goal would to also creat this instance with a [Packer](https://github.com/chef-training/chefdk-fundamentals-image) script similar to the other training instances.  It is based on a Marketplace AMI so it cannot be made public. If you would like access to this AMI to deliver training please contact [training@chef.io](mailto:training@chef.io) the request that includes your Amazon Account Id.

### Creating the Workstation

The workstation and the nodes are Windows 2012R2 instances with the following credentials:

```
user:     Administrator
password: Cod3Can!
```

The AMI was generated with the following actions:

> NOTE: The creation of the image is a manual process of executing a number of steps and then saving that current working instance as an image.

* [Change](https://support.managed.com/kb/a472/how-to-change-the-administrator-password-in-windows-server-2003-2008-r2-or-2012.aspx) the Administrator's password to "Cod3Can!"

* Enable PowerShell Script Execution

```
Set-ExecutionPolicy RemoteSigned
```

* Installed the basic components through the Nordstrom's ChefDK bootstrap script

```
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/Nordstrom/chefdk_bootstrap/master/bootstrap.ps1).Content | Invoke-Expression
```

* Installed the Atom FoodCritic Linter
* Installed the Atom Rubocop Linter

* Added an ec2 json hints file (content: `{}`) to `C:\chef\ohai\hints\ec2.json`

* Write a `kitchen-template.yml` to the "\\Users\\Administrator" that contains the following [content](https://github.com/chef-training/chef-essentials-windows/blob/master/kitchen-template.yml).

* Install the kitchen-ec2 gem

```
chef gem install kitchen-ec2
```

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
```
