# Multi-AZ VPC CloudFormation Template
This AWS CloudFormation template creates a Virtual Private Cloud (VPC) that provides the network backbone for a three-tier web application.  Components include public and private subnets spread over two Availability Zones; an Internet Gateway and redundant NAT Gateways; NACLs to secure ingress and outgress traffic to each subnet; and Security Groups for the web, application, and database tiers.  The architectural diagram below shows the resources created by the template.

https://github.com/smscully/Networking/blob/main/docs/VPC-Multi-AZ%20VPC.drawio.png

Before launching the templates, always review the resources that will be created, as some resources may not be free-tier elegible.

## Templates
The following templates . . .
+ **vpc.yaml:** Creates a multi-AZ VPC two public subnets for EC2 web servers, two private subnets for EC2 application servers, and two private subnets for MySQL databases.  Standard security components are also created, include an Internet Gateway, NAT Gateways, security groups, Network ACLs, and . . .
+ **instances.yaml:** Creates an application load balancer, auto scaling group, EC2 instances, and database instances. . .
+ **cloudwatch.yaml:** Creates CloudWatch events (?), alarms (?)

## Modification Notes
Some of the template code that lends itself to quick customization includes:
+ Route Table Entries: 

(should i create a cfn to create the launch template?, with parameter option to provide name of instance AMI?)
