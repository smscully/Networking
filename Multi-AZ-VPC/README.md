# Multi-AZ VPC CloudFormation Template
This AWS CloudFormation template creates a virtual private cloud (VPC) that provides the network backbone for a three-tier web application.  Components include public and private subnets spread over two Availability Zones; an internet gateway (IGW) and redundant NAT gateways; network access control lists (NACL) to secure inbound and outbound subnet traffic; and Security Groups for the instances in the web, application, and database tiers.  The architectural diagram below shows the network resources created by the template.

![VPC diagram](https://github.com/smscully/Networking/blob/main/docs/VPC-Multi-AZ%20VPC.drawio.png)

Before launching the template, always review the resources that will be created, as some resources may not be free-tier elegible.  In this case, for example, the NAT Gateways incur an hourly charge.  Please refer to the AWS website for specific regional pricing: [Amazon VPC Pricing](https://aws.amazon.com/vpc/pricing/)

## Template Overview
The vpc.yaml template creates the following types and number of AWS resources:

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
|EnvironmentName|Environment name used by CloudFormation to identify each resource.|??|
|VPCCIDR|CIDR block of the VPC.|10.0.0.0/16|
|PublicSubnetWeb1CIDR|CIDR block of the public web server subnet in AZ1.|10.0.10.0/24|
|PublicSubnetWeb2CIDR|CIDR block of the public web server subnet in AZ2.|10.0.11.0/24|
|PrivateSubnetApp1CIDR|CIDR block of the private app server subnet in AZ1.|10.0.20.0/24|
|PrivateSubnetApp2CIDR|CIDR block of the private app server subnet in AZ2.|10.0.21.0/24|

## Stack Output Values
Resource names and properties can be exported as stack output values.  Stacks in the same AWS account and region can import these values to reference or access the resources.  As an example, a subnet ID exported as an output value can be referenced as a launch destination for an EC2 instance created by another stack.  As shown below, this stack exports the names of the VPC, subnets, and security groups, which can then be referenced by other stacks that create EC2 instances, RDS DBs, or other resources.

|Output|Description|Example Value|
|---------|-----------|-------------|
|VPC|A reference to the created VPC|??|
|PublicSubnetWeb1|Reference to the public subnet for web servers in AZ1.|??|
|PublicSubnetWeb2|Reference to the public subnet for web servers in AZ2.|??|
|PrivateSubnetApp1|Reference to the private subnet for app servers in AZ1.|??|
|PrivateSubnetApp2|Reference to the private subnet for app servers in AZ2.|??|
|PrivateSubnetDB1|Reference to the private subnet for database servers in AZ1.|??|
|PrivateSubnetDB2|Reference to the private subnet for database servers in AZ2.|??|
|SecurityGroupWeb|Reference to the security group for web server instances.|??|
|SecurityGroupApp|Reference to the security group for app server instances.|??|
|SecurityGroupDB|Reference to the security group for RDS DB server instances.|??|

## Deployment Instructions

### Prerequisites
The template may be deployed using the the command line, which requires installation of the AWS CLI, or the console.

## Customization
Some of the template code that lends itself to quick customization includes:
+ **Specific AZs:** The template uses the instrinsic function FN::GetAZs to return an array of AZs in the region, and then selects the first two values in the array.  To use different AZs, either change the index reference to the array value, or replace function call with the explicit name of the AZ.

For example, replace the instrinsic !Select function calls to !GetAZs:
```yaml
AvailabilityZone: !Select [ 0, !GetAZs '' ]
```
With the AWS name of a specific AZ:
```yaml
AvailabilityZone: us-east-1a
```
+ DB Subnet Route Table Entries: Because RDS is a fully-managed service, the DB subnet route tables do not contain entries to the NAT gateways.  For self-managed databases, update each route table with a route to the NAT gateway in the appropriate AZ:
```yaml
PrivateSubnetDB1Route:
  Type: AWS::EC2::Route
  DependsOn: InternetGatewayAttachment
  Properties:
    RouteTableId: !Ref PrivateSubnetDB1RouteTable
    DestinationCidrBlock: 0.0.0.0/0
    NatGatewayId: !Ref NATGateway1
```
