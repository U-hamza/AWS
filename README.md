# AWS Assignments


## Assignment 1 Objective
Create a custom VPC with one public and one private subnet, set up the correct routing for internet access, and deploy EC2 instances across them.


## Architectural Diagram

<img width="735" height="539" alt="Screenshot 2026-05-24 at 11 19 22" src="https://github.com/user-attachments/assets/88fe1455-f0e2-49fc-a705-c285db35a88d" />

</br>


## 1. Create a VPC
AWS Console → search VPC → Your VPCs → Create VPC

The following was used to create the VPC:

- Resources to create: VPC only
- Name: custom-vpc (You can name this anything)
- IPv4 CIDR: 10.0.0.0/16
- Click create VPC


In this case the CIDR (Classless Inter-Domain Routing) 10.0.0.0/16 was used as the standard. (/16 not nessecary for smaller projects but used as standard for this project due to number of IP addresses it provides which is above 65,000)


## 2. Create Subnets

Go to Subnets → Create subnet



