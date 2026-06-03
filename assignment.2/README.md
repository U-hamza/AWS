
# Architectural Diagram


# Project Test Result 


https://github.com/user-attachments/assets/70fbbb65-3700-4955-a0a9-b5eb08b10610




# Assingment Layout

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




    






























# Challenges Faced & How I Overcame Them
- Designing the VPC and subnet structure – Initially, it was challenging to determine the correct CIDR ranges and subnet layout. I overcame this by carefully planning the network architecture, using separate public and private subnets across multiple Availability Zones to ensure proper segmentation and high availability.
- Configuring routing and internet access – The private EC2 instances were unable to access the internet due to an incorrect route table configuration that resulted in a blackhole route. I resolved this by troubleshooting the route tables, recreating the correct route to the NAT Gateway, and verifying that the Internet Gateway and NAT Gateway were configured correctly.
- Deploying web servers on private EC2 instances – The Apache installation initially failed because the instances could not reach the Amazon Linux repositories. After fixing the NAT Gateway and routing issues, I successfully deployed Apache using user-data scripts and verified that the web servers were running correctly.
- Troubleshooting unhealthy targets in the Target Group – Both EC2 instances initially failed the ALB health checks. I investigated the issue by reviewing system logs, confirming that Apache was installed and running, and ensuring the health check path and target group settings were configured correctly.
- Configuring secure access between components – Setting up the security groups correctly was essential. I ensured that the Application Load Balancer accepted public HTTP traffic while the EC2 instances only accepted HTTP traffic from the ALB security group, preventing direct public access to the servers.
- Resolving ALB 502 Bad Gateway errors – After deploying the infrastructure, the ALB returned 502 errors. I systematically checked the web server status, target group health, security group rules, and networking configuration until the underlying issues were identified and resolved.

This project strengthened my understanding of AWS networking, routing, security groups, NAT Gateways, load balancing, health checks, and troubleshooting cloud infrastructure issues in a production-style environment.
