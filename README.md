# AWS CI/CD Deployment to EC2

A demo Node.js application deployed using AWS CodePipeline, CodeBuild, and CodeDeploy on an EC2 instance.

## Services used:
- AWS CodePipeline
- AWS CodeBuild
- AWS CodeDeploy
- EC2 (t2.micro)
- IAM Role for deployment
STEP 1 — Launch EC2

Open AWS Console

Search EC2

Click Launch Instance

Choose:

Amazon Linux 2 (recommended)

Instance type: t2.micro (Free Tier eligible)

Create or select a Key Pair (Download .pem)

Network settings:

Allow SSH

Allow HTTP

Click Launch

STEP 2 — Connect to EC2 from Mac Terminal
chmod 400 your-key.pem
ssh -i your-key.pem ec2-user@<your-ec2-public-ip>
STEP 3 — Install Node & CodeDeploy agen
sudo yum update -y
sudo yum install nodejs -y

# Install CodeDeploy agent
sudo yum install ruby wget -y
cd /home/ec2-user
wget https://aws-codedeploy-ap-south-1.s3.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent start