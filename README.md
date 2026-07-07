# aws-ec2-nginx-alb-portfolio-site# AWS Portfolio Website on EC2 + Nginx + ALB

## Overview
This project documents the deployment of my personal portfolio website on AWS using Amazon EC2 and Nginx, then extending it with an Application Load Balancer (ALB). The goal of the project was to gain hands-on experience with core AWS infrastructure services while building a public-facing website.

## Architecture
Internet → ALB → EC2 (Amazon Linux + Nginx)

## AWS services used
- Amazon EC2
- Application Load Balancer (ALB)
- VPC
- Internet Gateway
- Security Groups
- Amazon Linux
- Nginx
- AWS Billing / Free Tier monitoring
- AWS WAF (tested and later removed for cost control)

## What I built
- Launched an EC2 instance running Amazon Linux
- Installed and configured Nginx to host a personal portfolio page
- Created a static HTML landing page for my GitHub / homelab portfolio
- Configured a public-facing ALB and target group to front the web server
- Verified target health and ALB access using the AWS-generated ALB DNS name
- Explored WAF attachment and CloudWatch logging considerations, then removed WAF to avoid unnecessary lab costs
- Reviewed AWS Free Plan / credits behavior and estimated monthly cost of the stack

## Current architecture
- **Web server:** Amazon EC2 t3.micro
- **Web service:** Nginx
- **Frontend:** static HTML portfolio page
- **Load balancing:** Application Load Balancer
- **Access path:** ALB DNS name → target group → EC2
- **Public access:** ALB is publicly reachable

## Key lessons learned
### 1. EC2 vs serverless / managed approaches
This project reinforced the basics of running a public web server on a VM: instance lifecycle, Linux package installation, Nginx service management, and security group configuration.

### 2. Internet Gateway vs NAT Gateway
I used this lab to understand the difference between an Internet Gateway and a NAT Gateway:
- Internet Gateway is required for public internet access to a public subnet / public-facing workload
- NAT Gateway is used for outbound-only internet access from private subnets
- NAT Gateway can incur hourly cost even when idle, so it should not be created casually in a small lab

### 3. ALB behavior and billing
I learned that an ALB can be used to front an EC2-hosted website and provide a more production-like architecture, but it also has hourly and LCU-based cost considerations. Even in a low-traffic personal lab, it’s important to understand what continues billing while resources remain provisioned.

### 4. WAF cost awareness
I briefly tested AWS WAF and reviewed WAF logging to CloudWatch, but removed it to avoid unnecessary cost in a small personal environment. This was a useful lesson in evaluating whether a managed security service is appropriate for a given stage of a project.

### 5. Free plan vs billable services
The AWS Billing console showed that my account is currently on the Free Plan with credits remaining. Even so, not every AWS service should be assumed “free,” and some services can still consume credits.

## Cost considerations
This project was also used to understand AWS cost behavior in a small lab environment.

### Resources that may incur ongoing cost
- EC2 instance runtime
- Public IPv4 address
- EBS root volume
- ALB hourly usage
- ALB LCU usage
- Optional services such as NAT Gateway or WAF if left running

### Cost control actions taken
- Avoided keeping NAT Gateway unnecessarily
- Removed WAF after testing
- Monitored Free Tier / credits in Billing
- Treated the account credits as a lab budget rather than assuming everything was free

## Screenshots to add
- EC2 instance details
- ALB listener / target group health
- Working website behind the ALB
- AWS Billing Free Tier / credits page
- Optional architecture diagram

## Future improvements
- Add custom domain + Route 53
- Add HTTPS using ACM on the ALB
- Restrict EC2 security group to allow traffic only from the ALB security group
- Move the web server to a private subnet behind the ALB
- Add a Lambda-backed contact form to the website
- Rebuild the stack with Infrastructure as Code (Terraform)

## Live access
Current ALB endpoint:
`http://ALB-Webserver-674461301.ap-southeast-1.elb.amazonaws.com`

## Repository contents
- `index.html` – portfolio landing page
- `README.md` – project documentation
- `screenshots/` – AWS console and website screenshots
- `diagrams/` – architecture images (optional)

## Why I built this
I’m using projects like this to strengthen my transition from enterprise networking into cloud and infrastructure engineering. My goal is not only to deploy services, but to understand how AWS networking, load balancing, cost, and security fit together in a practical environment.
