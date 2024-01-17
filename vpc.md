
# A VPC
- A VPC (Virtual Private Cloud) is a logically isolated network that you can create on AWS.
- It allows you to launch and control the access of your AWS resources, such as EC2 instances, within your own virtual network.
- To create a VPC, you need to specify the following parameters1:

	- The ``IP address range`` for your VPC, in CIDR notation. For example, 10.0.0.0/16.
	- The Availability Zones where you want to create subnets. Each subnet resides in one Availability Zone and can contain your AWS resources.
	- The ``internet connectivity`` options for your VPC. You can attach an internet gateway to enable communication between your VPC and the internet. You can also use a NAT gateway to allow your private subnet instances to access the internet without being reachable from the internet.
	- The ``name tags`` for your VPC and other VPC resources, such as subnets, route tables, and gateways. Name tags help you identify and organize your resources.

You can use:
	- the AWS console
	- the AWS CLI
	- or the AWS SDKs to create a VPC.
The easiest way to get started is to use the VPC wizard in the AWS console, which guides you through the steps to create a VPC with the most common configuration options1. Alternatively, you can create a VPC manually and customize it according to your needs2
