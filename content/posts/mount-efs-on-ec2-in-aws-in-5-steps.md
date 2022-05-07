---
title: "Mount EFS on EC2 instance in AWS in 5 steps"
categories:
  - Cloud
tags:
  - AWS
  - EFS
  - EC2
date: 2022-05-07
image: /webscraping.jpg
---
In this article we will see step-by-step process on mounting a [EFS](https://aws.amazon.com/efs/) file system on to an EC2 instance. There are lot of use cases on why we need an EFS file system. We have a Python [AWS Lambda](https://aws.amazon.com/lambda/) function which uses packages like numpy, scipy, pandas and scikit learn. When we pack these packages in to a zip file to deploy it exceed the size limit of 250k for lambda. So, we need to install these packages in an EFS file systems and make our Lambda function to get these packages from EFS. We need EC2 instance to mount EFS to install packages. 

{{< table_of_contents >}}

{{< adsense >}}

## Create an EC2 instance
Let's launch an EC2 instance with the following configuration (free tier eligible). Once you login into your AWS console, search for EC2 under services and go to EC2 dashboard. Please refer to this [link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-instance-wizard.html) if you are not familiar to launch an EC2 instance. I have used the following configuration:

*  I gave it a name - TestEFS
*  I am using Amazon Linux 2 AMI (HVM) 64 bit
*  Selected instance type t2.micro
*  I generated a new key pair. You can use an existing one if you have. AWS shows old keys in the drop down if you have any.
*  Keep the default network settings (allow traffic from anywhere for ssh)
*  Increase the disk size to 30 gb (free tier eligible)

Your instance will be created within a default VPC and a new security group. If you are using Linux machine, change the permissions of your key to `700` and try to login into your instance with the following commands:

```shell
$ chmod 700 testefs.pem
$ ssh -i testefs.pem ec2-user@<your instance public IP>
```

{{< adsense >}}


## Create an EFS file system
Login to your AWS console and search for EFS service. On the landing page of EFS, you will see `Create file system` option. Click on it to get started. You will see the following modal dialog to quickly create one.

![EFS create dialog](/efs-modal.jpg)

Give it a name and leave the rest as is. **Make sure the default VPC is selected. We need to have both EC2 instance and EFS on the same VPC**. For learning purposes we are using default VPC, but in the real world your DevOps may have a VPC setup for your project which you can select from the dropdown. It will take few minutes for EFS to get created. EFS is available is all the zones. You will see multiple IP addresses for the file system created for multiple availability zones, which are called as `Mount targets`.

![EFS mount targets](/efs-mount-targets.jpg)

If you don't want multiple mount targets (in my case I don't need) you can remove them.

{{< adsense >}}

## Update Security group
As you can see in the above image, we are using default security group, but your DevOps may create a new security group for you. For keeping things simple, we will use and update the default security group. We want to mount this EFS only on our EC2 instance which we created above. So we need to add an `Inbound rule` to the security group used by EFS to allow `NFS (Network File System)` connections from our EC2 instance. To do that we need the security group ID of our EC2 instance, which you can get from the `Security` tab of your instance details as shown below:

![EC2 security group](/ec2-security-group.jpg)

Copy the security group ID and create an inbound rule in the EFS security group by editing inbound rules as shown below:

![NFS inbound rule](/nfs-inbound-rule.jpg)

## Create an Access point
Now we need to create an access point so that we can mount it on EC2. You can create multiple access points for different paths of your EFS file system. For now we create one access point for root (/). Select the file system from the list and go to `Access points` tab and click on `Create access point`. You will see a screen as shown below:
![EFS create access point](/efs-create-access-point.jpg)

Give it a name and put '/' in the Root directory path. Leave the rest of the fields as is and submit. You will see your access point in the list as shown below:

![EFS access point list](/efs-access-point-list.jpg)

{{< adsense >}}

## Mount EFS on EC2
In the above image, you will see a `Attach` button on top right corner. Clicking on that will show a modal dialog like:

![EFS attach modal](/efs-attach-modal.jpg)

We will mount our EFS via IP (for me DNS didn't work). Make sure you select the same availability zone as your EC2 instance. Copy the command, login into your EC2 instance and create a folder called `efs` (change the command if you use a different folder name). Run the command shown in the above image. If you don't see any errors then your have successfully mounted the EFS. Now you can create files and folders in your EFS filesystem and can access them across different availability zones. Make sure you use `sudo` to create them.


## Conclusion
In this article we learnt how to mount EFS on to EC2 instance. In the coming articles we will learn how to connect EFS to AWS Lambda functions to avoid 250k size limitation. 