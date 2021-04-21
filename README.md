# IAC-Deploy-EC2-with-given-VPC-

Create an EC2 instance with a default VPC. The default VPC is selected to make the EC2 instance public so people can access the DNS link. 

### STEP 1 ###

Make sure to have a default VPC. Most of the users with an AWS account will have a default VPC. Go to the search bar and type VPC. It will take you to the VPC Dashboard. 
Note down VPC ID
Note down Subnet ID

### STEP 2 ###

Write a Cloudformation Script using yml. 
There are **Two parts to this challenge** 

The First one is deploying the SecurityGroup for the defualt VPC with unlimited outbound restrictions. 
The Second one is deplying a EC2 instance to that is attached to this VPC. 

### STEP 3 ###

```
  myWebAccessSecurityGroup:
    

    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow http to test host
      VpcId: copy paste your VPC ID
         
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIp: 0.0.0.0/0
      SecurityGroupEgress:
      - IpProtocol: -1
        FromPort: -1
        ToPort: -1
        CidrIp: 0.0.0.0/0
```

Make sure that outbound (SecurityGroupEgress) is set to -1. This means that all activities are allowed. 

### STEP 4 ### 

```
myWebServerInstance: 
    Type: AWS::EC2::Instance
    Properties: 
      ImageId: type your ami ID
      InstanceType: t3.micro
      NetworkInterfaces: 
      - AssociatePublicIpAddress: "true"
        DeviceIndex: "0"
        GroupSet: 
          - Ref: "type your Securitygroup name"
        SubnetId: your subnet to the defualt VPC
```
Copy and paste this Apache script.

```
UserData:
       Fn::Base64: !Sub |
         #!/bin/bash
         sudo yum update -y
         sudo yum install -y httpd
         sudo systemctl start httpd
         sudo systemctl enable httpd
```
After the Script is complete. Enter the following command in the terminal

```
aws cloudformation create-stack --stack-name vpc --region us-west-2 --template-body file://createVPC.yml
```
Then Check the CloudFormation console to see if the stack is created. Also go to the EC2 instance dashboard. Navigate to the instance that was created. 

### STEP 5 ###

Copy the public IP adress. 
Make sure to change https to http when you copy paste the  public IP adress in the browser. 

### STEP 6 ###

Expected outcome is a test page on the browser. 
