# IAC-Deploy-EC2-with-given-VPC-

Create an EC2 instance with a default VPC. The default VPC is selected to make the EC2 instance public so people can access the DNS link. 

### STEP 1 ###

Make sure to have a default VPC. Most of the users with an AWS account will have a default VPC. Go to the search bar and type VPC. It will take you to the VPC Dashboard. 

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
