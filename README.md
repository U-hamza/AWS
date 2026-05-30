# AWS Assignments


## Assignment 1 Objective
Create a custom VPC with one public and one private subnet, set up the correct routing for internet access, and deploy EC2 instances across them.


## Architectural Diagram

<img width="745" height="563" alt="Screenshot 2026-05-30 at 14 00 27" src="https://github.com/user-attachments/assets/c42dd6db-0fba-49b9-99ba-82973137495c" />


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

Go to VPC → Subnets → Create subnet

<img width="1417" height="460" alt="Screenshot 2026-05-30 at 14 13 18" src="https://github.com/user-attachments/assets/d6f35244-3aea-449d-90aa-4f07af31c379" />

</br>


Select:

- VPC: custom-vpc (Select the VPC that was made earlier)

Create a public subnet:

- Name: public-subnet (You can name it anything)
- Availability Zone: choose one, e.g. eu-west-2a (Select the closest zone to your country/city)
- CIDR: 10.0.1.0/24


After you have made a public subnet then create a private subnet. 

- Name: private-subnet
- Same VPC
- CIDR: 10.0.2.0/24

Click create subnet. Now you should have to subnets in your subnets sections. Private and a public one. 

<img width="358" height="183" alt="Screenshot 2026-05-30 at 14 24 39" src="https://github.com/user-attachments/assets/a04c41a0-16b6-4138-84ea-8ae4ab2686d2" />

</br>


## 3. Enable Public IP for Public subnet

Go to Subnets → select public-subnet

Actions → Edit subnet settings

<img width="254" height="347" alt="Screenshot 2026-05-30 at 14 27 20" src="https://github.com/user-attachments/assets/7ce53bd9-b724-47ee-bec9-698af8ed8b13" />
</br>




Tick:

- Enable auto-assign public IPv4 address
- Click save

<img width="961" height="188" alt="Screenshot 2026-05-30 at 14 28 00" src="https://github.com/user-attachments/assets/83ba9ad3-027a-495f-92d7-0044aecf67cf" />

</br>


## 4. Create Internet Gateway


Go to Internet Gateways → Create internet gateway

- Name: custom-igw (Name it anything)
- Click Create.

<img width="1416" height="474" alt="Screenshot 2026-05-30 at 14 32 50" src="https://github.com/user-attachments/assets/14e466d7-455b-4335-98cb-2228663ef5ab" />]

</br>


Then select the internet gatewate by ticking the box:

- Actions → Attach to VPC → choose your custom-vpc.

An Internet Gateway lets public subnet resources connect to the internet when routing and public IPs are configured.


## 5. 









