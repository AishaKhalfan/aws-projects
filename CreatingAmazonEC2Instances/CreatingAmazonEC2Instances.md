# Creating Amazon EC2 Instances and Connecting to them

- **Amazon EC2 Instances** are virtual servers that run on the AWS cloud platform.
	- They allow you to choose the CPU, memory, storage, and networking resources that suit your applications.
	- You can also select different purchasing options and root volume types for your instances.
	- Amazon EC2 Instances are scalable, secure, and easy to manage with AWS tools.

- AWS provides multiple ways to launch Amazon Elastic Compute Cloud (Amazon EC2) instance. 
	- AWS Management Console
	- AWS Command Line Interface (AWS CLI)

The following diagram illustrates the final architecture that you will build:

![Architecture](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/archi_diagram.png)
*Final architecture diagram depicting a bastion host and web server instance in a VPC.*

## Objectives
- Launch an EC2 instance by using the AWS Management Console.

- Connect to the EC2 instance by using EC2 Instance Connect.

- Launch an EC2 instance by using the AWS CLI.

## Accessing the AWS Management Console
- Login to your personal AWS account
- In the console home, the Recently visited resources will be shown

![my-console](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/aws-console.PNG)

# Task 1: Launching an EC2 Instance by using the AWS Management Console
In this task, you launch an EC2 instance by using the AWS Management Console.
- On the AWS Management Console, in the Search bar, enter and choose EC2 to open the Amazon EC2 Management Console.
![ec2-search](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/ec2-search.PNG)
- From the Launch instance dropdown list, choose Launch instance to open the Launch an instance menu.
![launch-instance](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/launch-instance.PNG)

## Step 1: Choose name and tags
- You use tags to categorize your AWS resources in different ways, such as by:
	- ``purpose``
	- ``owner``
	- ``environment``
- This categorization is useful when you have many resources of the same type; you can quickly identify a specific resource by its tags. Each tag consists of a key and a value, both of which you define.

When you name your instance, AWS creates a key-value pair. The key for this pair is Name, and the value is the name that you enter for your EC2 instance.

- In the Name and tags section, for Name, enter Khalfan host

 ![name-tags](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/name-tag.PNG)

## Step 2: Choose an AMI
In this step, you choose an Amazon Machine Image (AMI). An AMI includes the following:

	- A template for the root volume for the instance (for example, an operating system or an application server with applications)

	- Launch permissions that control which AWS accounts can use the AMI to launch instances

	- A block device mapping that specifies the volumes to attach to the instance when it is launched

The Quick Start list contains the most commonly used AMIs. You can also create your own AMI or select an AMI from the AWS Marketplace, an online store where you can sell or buy software that runs on AWS.

Your ``Khalfan Host`` will use Amazon Linux 2.

- In the Application and OS Images (Amazon Machine Image) section, for Quick Start, confirm that ``Amazon Linux`` is selected. Keep this selection.
![amazon-linux](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/amazon-linux.PNG)
Note: This option corresponds to Amazon Linux 2 AMI (HVM) as indicated in the Description. 

## Step 3: Choose an instance type
In this step, you choose an instance type, which determines the resources that will be allocated to your EC2 instance. Each instance type allocates a combination of virtual CPU, memory, disk storage, and network performance.

	- Instance types are divided into families, such as :
		- compute optimized
		- memory optimized
		- storage optimized.
	- The name of the instance type includes a family identifier, such as T3 and M5.
	- The number indicates the generation of the instance, so M5 is newer than M4.

Your application uses a t3.micro instance type, which is a small instance that can burst above baseline performance when it is busy. It is suitable for development and testing purposes and for applications that have bursty workloads.
![instance-type](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/instance-type.PNG)
- From the Instance type dropdown list, search for and choose t2.micro because its free tier eligible
![t2-micro](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/t2-micro.PNG)
 

## Step 4: Configure a key pair
Amazon EC2 uses public key cryptography to encrypt and decrypt login information. To log in to your instance, you must create a key pair, specify the name of the key pair when you launch the instance, and provide the private key when you connect to the instance.

- If you decide to use ``EC2 Instance Connect`` to log in to your instance, you do not require a key pair.
	- In the Key pair (login) section, from the Key pair name - required dropdown list, choose Proceed without key pair (Not recommended).
- We will use the SSH Connect to log in to our instance, so we require a key pair.
![key-pair1](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/key-pair1.PNG)
	- In the Key pair (login) section, from the Key pair name - required dropdown list, on the right click ``create new key-pair``
![key-pair2](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/key-pair1.PNG)
	- Add the ``key pair name``
	- Select ``Key pair type`` to be ``DSA``
	- Select ``.pem`` if on Linux and ``.ppk`` if on Windows based machine as the ``Private key file format`` 
	- Click ``create key pair``
![key-pair-aisha](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/key-pair-aisha.PNG)

**NOTE**: Save this key-pair you will use it to connect to the instance

## Step 5: Configure the network settings
You use this pane to configure networking settings.


The virtual private cloud (VPC) indicates which VPC you want to launch the instance into. You can have multiple VPCs, including different ones for development, testing, and production.

You launch the instance in a public subnet within the Default VPC network.

- In the Network settings section, choose Edit.

- From the VPC - required dropdown list, choose  Default VPC.
![default-vpc](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/default-vpc.PNG)

The ``Khalfan VPC`` was created using an AWS CloudFormation template during the setup process of your lab. This VPC includes 2 public subnets and 2 private subnets

- In the Subnet dropdown list, notice that Public Subnet is selected by default. Keep this default setting.

- In the Auto-assign public IP dropdown list, notice that Enable is selected by default. Keep this default setting.
![vpc-khalfan](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/detailed-vpc.PNG)
- In the Firewall (security groups) section, notice that Create security group is selected. Configure the following options:

	- For Security group name - required enter khalfanSG

	- For Description - required enter Permit SSH connections
![vpcsg](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/vpc-sg.PNG)

**A security group acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you associate one or more security groups with the instance. You add rules to each security group that allow traffic to or from its associated instances. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group.**

 ![detailed-vpc](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/detailed-vpc.PNG)

## Step 6: Add storage
You can use this step to add additional Amazon Elastic Block Store (Amazon EBS) disk volumes and configure their size and performance.

You launch the EC2 instance using a default 8 gibibyte (GiB) disk volume. This is your root volume (also known as a boot volume).

- In the Configure storage pane, keep the default storage configuration.

 ![configure-storage](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/configure-storage.PNG)

## Step 7: Configure advanced details
- Expand the Advanced details pane.
![advanced](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/advanced.PNG)

- From IAM instance profile dropdown list, choose Khalfan-Role.
![khalfan-role1](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/khalfan-role1.PNG)
![khalfan-role](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/khalfan-role.PNG)
**The Khalfan-Role profile grants permission to applications running on the instance to make requests to the Amazon EC2 service. This association of Role is required for the second half of this Project, where you use the AWS CLI to communicate with the Amazon EC2 service.**

- Leave the default settings for all the other values.

 

## Step 8: Launch an EC2 instance
Now that you have configured your EC2 instance settings, it is time to launch your instance.
![launch-instance-mwisho](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/launch-instance-mwisho.PNG)
- In the Summary section, review the instance configuration details displayed, and choose Launch instance.
![success-launch](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/success-launch.PNG)

- Choose View all instances.
![view-instance](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/view-instance.PNG)

![my-instance](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/my-instance.PNG)

# Task 2: Logging in to the Khalfan host
In this task, you use EC2 Instance Connect to log in to the Khalfan-host that you just created.

- On the EC2 Management Console, from the list of EC2 instances displayed, choose the  check box for the Khalfan-host instance. 
![khalfan-host](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/khalfan-host.PNG)
- Choose Connect.
![khalfan-host-connect](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/khalfan-host-connect.PNG)

- On the EC2 Instance Connect tab, choose Connect to connect to the Khalfan-host.
![connect-to-instance](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/connect-to-instance.PNG)
![connect-to-instance2](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/connect-to-instance2.PNG)

Now that you are connected to the khalfanhost, you can use the AWS CLI to call AWS services.
![connected](https://github.com/AishaKhalfan/aws-projects/blob/main/CreatingAmazonEC2Instances/images/connected.PNG)
 
**Note: If you prefer to use an SSH client to connect to the EC2 instance, see the guidance below to Connect to your Linux instance.**
### Connect to instance from terminal using SSH
- Open the terminal and switch to the directory where your .pem file is located and connect to the instance through the below commands
	- chmod 400 pemfilename.pem
	//here pemfilename is the file name you have given to .pem file

	- ssh -i "pemfilename.pem" ec2-user@public_ip

	- example in my case: ssh -i aisha.pem ec2-user@54.190.182.214
![ssh-connect]()


32. Run the following command:
```bash
aws ec2 describe-instances --instance-ids $INSTANCE --query 'Reservations[].Instances[].State.Name' --output text
```
![describe-instance]()

# CONGRATULATIONS !!!
	- You successfully launched and connected to an ec2 instance

### Which method should you use?

- Launch from the management console when you quickly need to launch a one-off or temporary instance.

- Launch by using a script when you need to automate the creation of an instance in a repeatable, reliable manner.

