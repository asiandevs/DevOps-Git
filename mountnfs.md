Step 1
Create an EFS file system.

Navigate to the AWS Console > EFS > Create File System


Step 2
Create an EC2 instance with Amazon Linux 2 as the AMI.

Step 3
Navigate back to EFS and choose the EFS you created and under Network, you will the EFS details. 

Seciruty Group:
EFS Network ---> Go to your Security Groups --> Choose the security group associated with your EFS.
Edit inbound rules --> Add rule --> Choose NFS [The Port range is 2049 ] + source will be your EC2 Security Group

Step 4
Now we will connect our instance to the EFS file system. Navigate back to your EC2 instance and SSH into it.

Once in your instance run the following command, this is the EFS Mount Help Utility and is part of the util package
sudo yum install -y amazon-efs-utils
Now create a mount point on our instance so we can connect to our EFS
sudo mkdir efs

Now back to EFS and select your EFS file system and click Attach. Execute command to the EC2 to attach a file system.


