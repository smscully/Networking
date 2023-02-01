# Multi-AZ VPC CloudFormation Template
This AWS CloudFormation template creates a Virtual Private Cloud (VPC) that provides the network backbone for a three-tier web application.  Components include public and private subnets spread over two Availability Zones; an Internet Gateway and redundant NAT Gateways; NACLs to secure inbound and outbound subnet traffic; and Security Groups for the instances in the web, application, and database tiers.  The architectural diagram below shows the network resources created by the template.

![VPC diagram](https://github.com/smscully/Networking/blob/main/docs/VPC-Multi-AZ%20VPC.drawio.png)

Before launching the template, always review the resources that will be created, as some resources may not be free-tier elegible.  In this case, for example, the NAT Gateways incur an hourly charge.  Please refer to the AWS website for specific regional pricing: [Amazon VPC Pricing](https://aws.amazon.com/vpc/pricing/)

## Template Overview
The template create the following AWS resources:

## List of Parameters

|Parameter|Description|Default Value|
|---------|-----------|-------------|
|VPCCIDR  |The CIDR block of the VPC.|10.0.0.0/16|

## Deployment Instructions

### Prerequisites
The template may be deployed using the the command line, which requires installation of the AWS CLI, or the console.

## Modification Notes
Some of the template code that lends itself to quick customization includes:
+ Route Table Entries: 
