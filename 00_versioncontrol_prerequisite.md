## Version Control System - VCS
Is used to Version Control the Source Code Changes
It is used to Track the source code changes.

### Centralized version control systems (CVCS)
In centralized version control, there is a single central repository where all the versions of the code are stored. 
Developers check out the code from this central server, make their changes, and then check the code back in. 
Examples like - (SVN)
![image](https://github.com/user-attachments/assets/3cd06437-9212-4063-a9c2-c84781ef0022)


So, all the code versions are stored in one place - This means that if the central server goes down, no one can commit changes or even get the latest code.
		
### Distributed version control systems:

Distributed version control systems, like Git and Mercurial, work differently. Instead of a single central repository, every developer has a complete copy of the entire repository, including its history, on their local machine.

So, everyone has the full codebase and its history on their own computer. This makes DVCS more robust because developers can work offline, commit changes locally, and only need to connect to a central repository when they're ready to share their changes with others.

![image](https://github.com/user-attachments/assets/1f8f52c7-ea4d-4096-bb70-fb028cd9eed9)

		
#### How do developers share their changes if they all have their own copies?
In a DVCS, developers can push their changes to a shared repository, often called a remote repository, and pull changes from others. This allows for multiple workflows, including collaborative development models like feature branching and pull requests. [ Someone need to validate and authorize] 

So, in centralized systems, everyone depends on the central server, whereas in distributed systems, everyone has their own copy and can work independently.

CVCS is simpler and can be easier to manage for smaller projects or teams, but DVCS offers more flexibility, robustness, and better support for large, distributed teams.

## Pre Requisite
You can also use your windows machine. In that case you need to use GitBash to work with Git. Otherwise you can provision a Linux Instance.

### Launch an instance ( in my case I am using AWS EC2 Instance) 

https://github.com/asiandevs/cloud_services/blob/main/create_ec2.md

### Connecting to an EC2 Linux instance
https://github.com/asiandevs/cloud_services/blob/main/connect_EC2.md



