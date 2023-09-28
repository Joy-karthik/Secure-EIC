# Secure-EIC
This repository have a document of the secure EIC feature


#Create the secure EIC connection

    1. Begin by establishing a secure endpoint.
    2. Next, create two security groups: one for the private instance and another for the endpoint connection
    3. In the security endpoint, remove all inbound rules and only allow outbound traffic.
    4. If you're launching a Linux-oriented AMI, configure it to use port 22 with the destination redirected to the instance's security group.
    If you're launching a Windows-oriented AMI, set it to use port 3389 with the destination also redirected to the instance's security group.



    ![image](https://github.com/Joy-karthik/Secure-EIC/assets/75682653/e49158bd-f210-45a7-bdd2-8441ca909010)
    

   5.  Create another security group for the private instance. In the inbound rules, set up two rules: one for SSH port access with your specific IP and another for open access on any port, with the destination redirected to the EIC security group.

      ![image](https://github.com/Joy-karthik/Secure-EIC/assets/75682653/8955a5ab-2cad-4553-8c13-0d0987d6fbde)


    6. Finally, create the private instance without a public IP and without a key pair, and use the VPC created by EIC.







1. Check the connection--->curl http://169.254.169.254/latest/meta-data/instance-id
2. Connect to the ubuntu machine--->aws ec2-instance-connect ssh --instance-id i-0fd65704b62621f3c --os-user ubuntu
3. Connect to the Linux ami--->aws ec2-instance-connect ssh --instance-id i-0fd65704b62621f3c
4. Open tunnel creation--->aws ec2-instance-connect open-tunnel --instance-id i-0fd65704b62621f3c --remote-port 22 --local-port 5555


Use this custom policy for the EIC connection


#First_Apperance-->Give some authorization for the user

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ec2-instance-connect:OpenTunnel",
                "ec2-instance-connect:SendSSHPublicKey",
                "ec2:DescribeInstances",
                "ec2-instance-connect:SendSerialConsoleSSHPublicKey",
                "ec2:DescribeInstanceConnectEndpoints"
            ],
            "Resource": "*"
        }
    ]
}


#Second Apperance--->Remove the authorization only access the instance


{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ec2-instance-connect:OpenTunnel",
                "ec2-instance-connect:SendSSHPublicKey",
                "ec2-instance-connect:SendSerialConsoleSSHPublicKey".
            ],
            "Resource": "*"
        }
    ]
}


#Third Apperance--->Specific instance appear but this policy you might be get a error. so use 4th policy

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ec2-instance-connect:OpenTunnel",
                "ec2-instance-connect:SendSSHPublicKey",
                "ec2-instance-connect:SendSerialConsoleSSHPublicKey"
            ],
            "Resource": "arn:aws:ec2:ap-south-1:744096931876:instance/i-0e110b0469e3c0a8c"
        }
    ]
}


#4th Apperance-->This policy one access the specfic instnace and allow the all endpoint

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ec2-instance-connect:OpenTunnel",
                "ec2-instance-connect:SendSSHPublicKey",
                "ec2-instance-connect:SendSerialConsoleSSHPublicKey"
            ],
            "Resource": [
                "arn:aws:ec2:ap-south-1:744096931876:instance/i-0e110b0469e3c0a8c",
                "arn:aws:ec2:ap-south-1:744096931876:instance-connect-endpoint/*"
            ]
        }
    ]
}


#Fifth Apperance--->This is a full of restriction policy allow the specfic endpoint and specfic connection

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ec2-instance-connect:OpenTunnel",
                "ec2-instance-connect:SendSSHPublicKey",
                "ec2-instance-connect:SendSerialConsoleSSHPublicKey"
            ],
            "Resource": [
                "arn:aws:ec2:ap-south-1:744096931876:instance/i-0e110b0429343c0a8c",
                "arn:aws:ec2:ap-south-1:744096931876:instance-connect-endpoint/eice-03cec2d7947718afb"
            ]
        }
    ]
}
