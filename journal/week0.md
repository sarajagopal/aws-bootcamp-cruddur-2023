# Week 0 — Billing and Architecture
## Required HomeWork

### Install AWS CLI
I was able to install aws cli using the script added to gitpod.yml. 
Committed the code in the file gitpod.yml file.Initially made a mistake on the script and when i load the gitpod again, i noticed script is failing to install the aws. Then Fixed the issue and its working successfully .
Recommitted the code with the corrections.
Executed the cli command to verify the configuration completed.
![install AWS CLI Output](assets/week0-aws-cli.JPG)

### AWS Billing Alarm
I have created a Billing alarm for $10 using cloud watch. If there is any resources usaged crossed $10 then alarm will be generated to my mail id 
![Image of Cloud Watch Billing Alarm](assets/Creating-Alarm.JPG)

### AWS Budget
I have created a buget for $1 .Since it is minimal value i have created single budget inorder to avoid cost for creating more than 2 budget.Since budget value is low did not create any threshold inbetween .
![Image of Buget Alarm I have created](assets/Aws-buget.JPG)
## HomeWork Challengs.

