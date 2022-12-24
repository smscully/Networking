# Secure Multi-Tier Application Using CloudFormation
This sample application uses AWS CloudFormation templates to create a resource stack for a secure multi-tier, load balanced application with auto scaling.  Before launching the templates, always review the resources that will be created, as some resources may not be free-tier elegible.

## Templates
The following templates . . .
+ **vpc.yaml:** Creates a multi-AZ VPC two public subnets for EC2 web servers, two private subnets for EC2 application servers, and two private subnets for MySQL databases.  Standard security components are also created, include an Internet Gateway, NAT Gateways, security groups, Network ACLs, and . . .
+ **instances.yaml:** Creates an application load balancer, auto scaling group, EC2 instances, and database instances. . .
+ **cloudwatch.yaml:** Creates CloudWatch events (?), alarms (?)

## Notes

(should i create a cfn to create the launch template?, with parameter option to provide name of instance AMI?)
