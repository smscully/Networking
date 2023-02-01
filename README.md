# Multi-AZ VPC CloudFormation Template
This AWS CloudFormation template creates a Virtual Private Cloud (VPC) that provides the network backbone for a three-tier web application.  Components include public and private subnets spread over two Availability Zones; an Internet Gateway (IGW) and redundant NAT Gateways; Network Access Control Lists (NACL) to secure inbound and outbound subnet traffic; and Security Groups for the instances in the web, application, and database tiers.  The architectural diagram below shows the network resources created by the template.

![VPC diagram](https://github.com/smscully/Networking/blob/main/docs/VPC-Multi-AZ%20VPC.drawio.png)

Before launching the template, always review the resources that will be created, as some resources may not be free-tier elegible.  In this case, for example, the NAT Gateways incur an hourly charge.  Please refer to the AWS website for specific regional pricing: [Amazon VPC Pricing](https://aws.amazon.com/vpc/pricing/ target="_blank")

## Template Overview
The vpc.yaml template create the following type and number of AWS resources:

+ VPC (1)
+ Public Subnets (2)
+ Private Subnets (4)
+ NACL (6)
+ Security Groups (3)
+ IGW (1)
+ NAT Gateways (2)

## List of Parameters
The template parameters, listed below, can be modified as necessary to fit within the design of the AWS account.  The CIDR block ranges, for example, can be changed if required to avoid matching or overlapping blocks when peering VPCs.  The default parameter values can be updated directly in the CloudFormation template, through the console when creating the stack, or via customizing the parameter.json file if using the AWS CLI.

|Parameter|Description|Default Value|
|---------|-----------|-------------|
|EnvironmentName|The environment name used by CloudFormation to identify each resource.|??|
|VPCCIDR  |The CIDR block of the VPC.|10.0.0.0/16|
|PublicSubnetWeb1CIDR|The CIDR block of the public web server subnet in AZ1.|10.0.10.0/24|
|PublicSubnetWeb2CIDR|The CIDR block of the public web server subnet in AZ2.|10.0.11.0/24|
|PrivateSubnetApp1CIDR|The CIDR block of the private app server subnet in AZ1.|10.0.20.0/24|
|PrivateSubnetApp2CIDR|The CIDR block of the private app server subnet in AZ2.|10.0.21.0/24|
|PrivateSubnetDB1CIDR|The CIDR block of the private database server subnet in AZ1.|10.0.30.0/24|
|PrivateSubnetDB2CIDR|The CIDR block of the private database server subnet in AZ2.|10.0.31.0/24|

## Deployment Instructions

### Prerequisites
The template may be deployed using the the command line, which requires installation of the AWS CLI, or the console.

## Modification Notes
Some of the template code that lends itself to quick customization includes:
+ Route Table Entries: 
