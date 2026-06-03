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


## 5. Create Public Route Table

Go to Route Tables → Create route table

- Name: public-rt (Use any name but choose something that you can differentiate between easily)
- VPC: custom-vpc (Choose your custom VPC)

After you have created the public route table: 

- Open public-rt → Subnet associations → Edit subnet associations
- Tick: public-subnet
- Save

<img width="269" height="272" alt="Screenshot 2026-05-30 at 14 43 10" src="https://github.com/user-attachments/assets/ef8a1bff-b1ba-4d7e-9cb1-62a44b0cbe15" />

</br>


Now go to Routes → Edit routes → Add route

<img width="1260" height="205" alt="Screenshot 2026-05-30 at 14 44 48" src="https://github.com/user-attachments/assets/5b317a15-12b1-4656-b3cb-eeec399e4568" />
</br>



## 6. Create Nat Gateway

Go to NAT Gateways → Create NAT Gateway

The following was used:
- Name: custom-nat
- Subnet: public-subnet
- Connectivity type: Public
- Elastic IP allocation ID: click Allocate Elastic IP
- Click Create NAT Gateway.

A public NAT Gateway must be in a public subnet and uses an Elastic IP.


<img width="1045" height="762" alt="Screenshot 2026-05-21 at 19 19 09" src="https://github.com/user-attachments/assets/9458efef-82ae-4c19-925e-baa24d7e2b45" />

</br>


## 7. Create Private Route Table

Go to Route Tables → Create route table

The steps are the similar to creating the public route table. 

- Name: private-rt
- VPC: custom-vpc
- Open private-rt → Subnet associations → Edit subnet associations
- Tick: private-subnet
- Save


Then add route to nat gateway:
- Go to Routes → Edit routes → Add route
- Before you add the route make sure your custom nat gateway is selected. 

<img width="926" height="204" alt="Screenshot 2026-05-30 at 14 55 56" src="https://github.com/user-attachments/assets/232a7986-200c-463d-a1be-52fc3900f21d" />
</br>


Private subnet traffic can now go out to the internet through NAT, but the internet cannot directly start connections into the private instance.


## 8. Create Security Groups

You will have to create a private and a public one.

Go to EC2 → Security Groups → Create security group

For a Public EC2 SG:
- Name: public-ec2-sg
- VPC: Choose your VPC

Inbound rules should be as following:

<img width="692" height="459" alt="Screenshot 2026-05-30 at 15 07 28" src="https://github.com/user-attachments/assets/b4646cab-0e03-4af9-9912-929455f28912" />

</br>


Create a Private EC2 SG now using the same steps. (name it private-ec2-sg etc). Also the inbound rules will be different.
- Select custom and then click on your source and choose your public sg for both SSH and HTTP

<img width="696" height="209" alt="Screenshot 2026-05-30 at 15 13 59" src="https://github.com/user-attachments/assets/ba7e1ef9-a2ba-4985-bf4c-cb36dbd264bb" />

</br>

Security groups control allowed inbound and outbound traffic for EC2 instances.



## 9. Launch Public EC2 Instance

Go to EC2 → Instances

If it your first time launching an instance then step by step walkthroughs are available on the website. 

For this assignment the following was used:
- Name: public-ec2
- AMI: Amazon Linux 2023
- Instance type: t2.micro or t3.micro
- Key pair: create or select one
- VPC: custom-vpc
- Subnet: public-subnet
- Auto-assign public IP: Enable
- Security group: public-ec2-sg
- Launch Instance
- <img width="925" height="697" alt="Screenshot 2026-05-31 at 15 06 12" src="https://github.com/user-attachments/assets/91de22f1-3a84-4504-8f42-35de8334aefc" />

</br>



NOTE: Key pairs are crucial for keeping your instances safe and allowing it to run only run when the key is used. When you create a key pair it will download to your local machine. Do not re-download as you could face difficulties later when trying to use it. This key will then be used later when trying to run your EC2 instance. Also, know where the key pair is saved so you can easily locate it. 



## 10. Launch a private EC2 Instance

Similar to the other instance, launch a private one now. 

- Name: private-ec2
- AMI: Amazon Linux 2023
- Instance type: t2.micro or t3.micro
- Key pair: use the same key pair
- VPC: custom-vpc
- Subnet: private-subnet
- Auto-assign public IP: Disable (This is very important as we do not want any inbound traffic to the private EC2 instance)
- Security group: private-ec2-sg
- Launch instance



## 11. Connect & Test Public EC2 

You can use the aws console terminal or your local machine. 

The following code needs to be used to allow access and modifications.

First run this: 
- chmod 400 your-key.pem

Then this: 
- ssh -i your-key.pem ec2-user@PUBLIC_EC2_PUBLIC_IP

your-key.pem is the key pair that was saved earlier. Also, these commmands are to be used only if you're located within the key pair directory. If not then use the root paths but if you cd into your directory (where the key pair is) then use the above. 

Your Public EC2 - Public IP can be found by going to instances and clicking on the public-EC2. It will then show all the details below in your instance. 

Test the instance on your terminal: 
- use: curl https://amazon.com

If your instance is running properly then you should see HTML script on your screen. 


## 12. Connect & Test Private EC2

You can only access the private EC2 via the public EC2.

The following was done: 
-cd ~/Downloads  (The key pair file was located in Downloads)
- ssh-add key.pem
- ssh -A -i key.pem ec2-user@PUBLIC_EC2_PUBLIC_IP

This will then add the key pair to the public EC2 which allow you to SSH into the private EC2. 

Use this to access private instance:
ssh ec2-user@private_ip

This should then direct you to your private EC2. You will see the EC2 promt will change when you enter your private ec2. On your terminal the ID's for the istances will change indicating that you have accessed the private instance. 



Finally test it using:
Curl https://amazon.com


If this returns a html script/page then your private EC2 instance is running fine and your private EC2 has outbound internet through the NAT Gateway.

<img width="451" height="122" alt="Screenshot 2026-05-31 at 15 56 25" src="https://github.com/user-attachments/assets/cfaeea8e-8751-4b10-a599-8c3c4889e742" />

</br>



## Challenges
During this project, I encountered several challenges while designing and configuring the AWS network infrastructure. These included troubleshooting SSH connectivity issues, managing EC2 key pairs, configuring route tables correctly, and ensuring the NAT Gateway provided outbound internet access to the private instance. I also gained a deeper understanding of VPC networking concepts such as CIDR ranges, subnet design, security groups, and the relationship between Internet Gateways and NAT Gateways. Working through these challenges strengthened my troubleshooting skills and improved my confidence in building secure and scalable AWS environments.
