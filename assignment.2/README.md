
# Architectural Diagram

<img width="602" height="570" alt="Screenshot 2026-06-13 at 17 40 10" src="https://github.com/user-attachments/assets/943b5f4a-267b-417d-8d6e-4d80bbb27b90" />



# Project Test Result 


https://github.com/user-attachments/assets/70fbbb65-3700-4955-a0a9-b5eb08b10610




# Assignment Layout

1. Create the VPC
2. Create public and private subnets
3. Create route tables, Internet Gateway, and NAT Gateway (if using private subnets)
4. Create Security Groups
5. Launch the EC2 instances
6. Create the Target Group
7. Create the ALB
8. Register the instances with the Target Group
9. Test



<img width="300" height="300" alt="Screenshot 2026-06-03 at 22 22 09" src="https://github.com/user-attachments/assets/8b985afe-7d47-4e6f-bc69-22dd4942378a" />

</br>



# Public and Private Subnets
Two subnets for private and public were set up in two different availability zones. This was one of the main goals for the project. The two private sunbets were created for the two EC2 instances and the two public ones for the ALB. 

Public Subnet 1:  10.0.1.0/24
Public Subnet 2:  10.0.2.0/24

Private Subnet 1: 10.0.3.0/24
Private Subnet 2: 10.0.4.0/24

CIDR used for the subnets.



# Internet Gateway/ Nat Gateway
IG: An internet gateway was created and attached to the VPC. This step is crucial otherwise the ALB cannot recieve traffic from the user and NAT gateway will not be able to access the internet.

NAT Gateway:
This was setup in one of the private subnets. 



# Private and Public Route Tables

Public: 
The public route table needs to be in the correct VPC. It needs to be connected with the Internet Gateway and destination should be 0.0.0.0/0 to allow traffic from all users. 

Both public subnets need to be associated with the public route table. 


Private: 
Needs to be in the VPC and connected to the NAT Gateway. Both private subnets should be associated with the route table and this is where both of the EC2 instances will be located. 



# Security Groups 
One ALB and one EC2 security group is needed. 

- ALB-sg: inbound rules were set to HTTP over port 80 and Source: 0.0.0.0/0
- EC2-sg: inbound rules were set to HTTP over port 80 and Source: ALB-sg


# EC2 Instances
Two EC2 instances were created in two different private subnets in different availability zones.

- EC2-1: was made in the custom vpc using private-subnet 1, and using EC2-sg. As this is in a private subnet auto-assign public IP needs to be off. User-script was inserted in the additional options section.

- EC2-2: in the custom VPC using public-subnet 2, and using EC2-sg. The same user-script was inserted except the html line which was different.

<img width="554" height="597" alt="Screenshot 2026-06-07 at 15 05 11" src="https://github.com/user-attachments/assets/21a30928-6d64-4320-8b2f-d735524ca5b4" />




# Target groups
A target group was made using the following:
- Target type: Instances
- Protocol: HTTP
- Port: 80
- VPC: Same VPC as EC2 instances
- Health check path: /

Then both EC2 instances were registered with the target group. When both are registered ensure the health status is healthy for both. 

<img width="1141" height="299" alt="Screenshot 2026-06-02 at 18 26 20" src="https://github.com/user-attachments/assets/fbc118a9-4d49-47cb-9037-f5538a501ca6" />




# Application Load Balancer
When you go onto load balancers, a few of other options will also display. For this assignment an ALB was used.
The following configurations were used for the ALB:

- Scheme: Internet-facing
- IP address type: IPv4
- VPC: Same VPC
- Subnets: The two public subnets in different AZs
- Security Group: ALB SG

Listner was over:
- HTTP: 80
- Forward to: your target group.



# Testing
After all the above was correct and health checks were healthy, the ALB DNS name was run in the browser using:

http://your-alb-dns-name

If the html text from the user-script was displaying then the infrastructure had been deployed correctly. Also, refreshing should alternate between the two user scripts and display different text. 


    




# Challenges Faced & How I Overcame Them
- Configuring routing and internet access – The private EC2 instances were unable to access the internet due to an incorrect route table configuration that resulted in a blackhole route. I resolved this by troubleshooting the route tables, recreating the correct route to the NAT Gateway, and verifying that the Internet Gateway and NAT Gateway were configured correctly.
- Deploying web servers on private EC2 instances – The Apache installation initially failed because the instances could not reach the Amazon Linux repositories. After fixing the NAT Gateway and routing issues, I successfully deployed Apache using user-data scripts and verified that the web servers were running correctly.
- Troubleshooting unhealthy targets in the Target Group – Both EC2 instances initially failed the ALB health checks. I investigated the issue by reviewing system logs, confirming that Apache was installed and running, and ensuring the health check path and target group settings were configured correctly.
- Configuring secure access between components – Setting up the security groups correctly was essential. I ensured that the Application Load Balancer accepted public HTTP traffic while the EC2 instances only accepted HTTP traffic from the ALB security group, preventing direct public access to the servers.
- Resolving ALB 502 Bad Gateway errors – After deploying the infrastructure, the ALB returned 502 errors. I systematically checked the web server status, target group health, security group rules, and networking configuration until the underlying issues were identified and resolved.

This project strengthened my understanding of AWS networking, routing, security groups, NAT Gateways, load balancing, health checks, and troubleshooting cloud infrastructure issues in a production-style environment.
