 ## **Automation with AWS CloudFormation**
Deploying and Managing Infrastructure as Code

## **Overview**

In this lab, I learned to automate infrastructure deployment using AWS CloudFormation, a service that enables you to model, provision, and manage AWS resources using declarative templates.

I explored how to:

Deploy and manage CloudFormation stacks

Define infrastructure using YAML templates

Add and update resources such as VPCs, Security Groups, S3 Buckets, and EC2 Instances

Delete stacks and automatically remove resources

This lab demonstrates how Infrastructure as Code (IaC) ensures consistency, repeatability, and reliability, minimizing human error and deployment drift.

## **Objectives & Learning Outcomes**

By the end of this lab, I was able to:

Deploy a CloudFormation Stack with a VPC and Security Group
Add and configure resources such as Amazon S3 and EC2 using YAML templates
Reference parameters and resources using the !Ref intrinsic function
Use AWS Systems Manager Parameter Store to dynamically fetch the latest AMI
Safely delete stacks and associated resources
Understand the power of Infrastructure as Code for automation and scalability

## **Architecture Diagram**
<img width="900" height="500" alt="511745bf-c45a-450e-9a2d-5fa6d9a7ef49" src="https://github.com/user-attachments/assets/367dbcdf-456c-4c6e-9ba8-741578531389" />

Architecture Description:

CloudFormation Stack defines and manages all resources.

VPC + Security Group form the network layer.

S3 Bucket provides object storage.

EC2 Instance (App Server) deployed within VPC, referencing AMI via SSM Parameter Store.

CloudFormation Template (YAML) orchestrates deployment, update, and deletion of resources automatically.

## **Command and Steps (Bash & Yaml)**
```bash
# Launch a CloudFormation Stack
aws cloudformation create-stack \
  --stack-name Lab \
  --template-body file://task1.yaml

# Check Stack Creation Progress
aws cloudformation describe-stacks --stack-name Lab

# Update the Stack to Add an S3 Bucket
aws cloudformation update-stack \
  --stack-name Lab \
  --template-body file://task2.yaml

#  Update the Stack Again to Add EC2 Instance
aws cloudformation update-stack \
  --stack-name Lab \
  --template-body file://task3.yaml

#  Delete the Stack and All Resources
aws cloudformation delete-stack --stack-name Lab
```

```Yaml
# VPC & Security Group
Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16

  AppSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow HTTP and SSH
      VpcId: !Ref VPC

# Add an S3 Bucket
  AppBucket:
    Type: AWS::S3::Bucket

# Add an EC2 Instance
 Parameters:
  AmazonLinuxAMIID:
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2

Resources:
  AppServer:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref AmazonLinuxAMIID
      InstanceType: t3.micro
      SecurityGroupIds:
        - !Ref AppSecurityGroup
      SubnetId: !Ref PublicSubnet
      Tags:
        - Key: Name
          Value: App Server
```

### Task 1 – Deploy the Stack

Uploaded task1.yaml to CloudFormation

Created a new stack named Lab

Observed the creation of a VPC and Security Group

Verified CREATE_COMPLETE status

### Task 2 – Add S3 Bucket

Edited template to include an S3 bucket resource

Updated stack → saw UPDATE_IN_PROGRESS → UPDATE_COMPLETE

Verified bucket in S3 console

### Task 3 – Add EC2 Instance

Added parameter for AmazonLinuxAMIID via SSM

Added App Server EC2 instance resource

Updated stack successfully

Verified instance in EC2 console

### Task 4 – Delete the Stack

Executed Delete stack in CloudFormation

Stack removed along with all resources (VPC, EC2, S3)


## **Screenshots**

Stack creation progress showing VPC and Security Group.
<img width="1654" height="400" alt="createstake comnplete" src="https://github.com/user-attachments/assets/da6a9971-1da1-4216-b49d-a692eda98186" />

Updated stack reflecting the new S3 bucket resource.
<img width="1873" height="400" alt="s3 bucket added" src="https://github.com/user-attachments/assets/199c1d73-6a35-47fe-9eb2-d8961f529b2c" />

EC2 instance successfully launched via CloudFormation.
<img width="1687" height="400" alt="AppServerDeployed" src="https://github.com/user-attachments/assets/f2e338f0-36dc-4711-8c16-f0b80c03b0ac" />

Stack deletion process with resources being cleaned up.
<img width="1713" height="400" alt="DeleteStackConfirmation" src="https://github.com/user-attachments/assets/343cdd0a-0145-4a0f-98df-f2bc70b13a6d" />


## **Key Takeaways**

Infrastructure as Code (IaC) eliminates manual configuration and ensures consistency.

CloudFormation templates enable version-controlled, repeatable deployments.

Stack updates simplify infrastructure scaling and modification.

!Ref and Parameters allow reusable and dynamic resource definitions.

Deleting a stack safely cleans up all dependencies automatically.


## **What Actually Happened**

Defined Infrastructure in a YAML CloudFormation template (VPC, Security Group).

Deployed the stack, verifying the creation flow and dependencies.

Extended the template with an S3 bucket and EC2 instance using CloudFormation update.

Used Parameters and !Ref for dynamic linking between resources.

Tested and validated resource creation via AWS Console.

Deleted the stack, demonstrating clean teardown automation.


## **Author:**

Amarachi Emeziem

Tools Used: AWS CloudFormation, EC2, S3, IAM, VPC, YAML, bash, console

LinkedIn profile: https://www.linkedin.com/in/amarachilemeziem/ 
