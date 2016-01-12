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

* Windows 2012R2

The AMI was generated with the following actions:

* Installed the basic components through the Nordstrom's ChefDK bootstrap script

```
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/Nordstrom/chefdk_bootstrap/master/bootstrap.ps1).Content | Invoke-Expression
```

* Enabled powershell script execution

* Changed the password to "Cod3Can!"

* Installed the Atom FoodCritic Linter
* Installed the Atom Rubocop Linter
* Write a `kitchen-template.yml` to the "\\Users\\Administrator"
